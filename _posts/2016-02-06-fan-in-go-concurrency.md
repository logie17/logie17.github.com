---
layout: post
title: "Go Concurrency Part 4"
description: "Go Concurrency"
category: programming
tags: [Go Golang Blogging Books Writing 2016 Concurrency]
---
{% include JB/setup %}
## Cool off with a Fan-In

A fan-in, not a new way to keep cool, but a conventional go routine pattern when a
programmer desires to merge multiple concurrent go channels into a single channel. This
tactic is often called
["multiplexing"](https://en.wikipedia.org/wiki/Multiplexing).

#### When is a fan-in needed?

A very common application is to have multiple go routines or workers processes operate on some
data and then merge that data into a single channel, this is a "fan-in".  There are two practical
approaches when writing fan-in code. One way is to have a channel that is shared among parallel
go routines; the routines will send data to this channel. The alternate approach or practice is that
each parallel routine writes to its own channel with the expectation that another routine will merge
the channels for processing. We will briefly explore each approach.

#### Let's fan out and get started

I actually mean fan-in, so let's pretend we have an array of interesting Mark Twain quotes that that we want to
process randomly:

        var twainQuotes = []string {
                "A man cannot be comfortable without his own approval.",
                "Always acknowledge a fault. This will throw those in authority off their guard and give you an opportunity to commit more.",
                "Always do right. This will gratify some people and astonish the rest.",
                "Courage is resistance to fear, mastery of fear - not absence of fear.",
                "Get your facts first, and then you can distort them as much as you please.",
                "Grief can take care of itself, but to get the full value of a joy you must have somebody to divide it with.",
        }

Next we create a twainQuoter that will randomly pick a quote and send it down a channel. Also, let's make
the function a little more interesting by throwing in a sleep that will sleep for a random amount of millisecond
time.

        func twainQuoter(ch chan string) {
                for {
                        ch <- twainQuotes[rand.Intn(len(twainQuotes))]
                        waitTime := (time.Duration(100 * rand.Intn(10)) * time.Millisecond)
                        time.Sleep(waitTime)
                }
        }


Finally, as we can see below, we will create a "reader" function that will listen to the out channel and output
the results.

        func reader(out chan string) {
                for x := range out {
                        fmt.Println(x)
                }
        }

        func main() {
                ch := make(chan string)
                out := make(chan string)
                go twainQuoter(ch)
                go twainQuoter(ch)
                go reader(out)
                for q := range ch {
                        out <- q
                }
        }

In the example we have two Mark Twain quoters, twainQuoters, which run in parallel and are writing to a shared channel. In order
to "fan-in" we can create another go routine to to range over the results of the shared channel, ie, the "reader" function.
The output of the program will look something like this:

        ...
        Get your facts first, and then you can distort them as much as you please.
        Grief can take care of itself, but to get the full value of a joy you must have somebody to divide it with.
        ...

#### Hold your remotes, what about multi channel merging?

However, as mentioned earlier, there is another pattern where various parallel routines maybe writing to multiple channels.
In this situation we need to concoct of a way to conflate the channels. There is an excellent write-up on how this could
be done in the [pipelines article](https://blog.golang.org/pipelines) on the golang blog. For the purposes of this article,
let's use a very similar example from the blog for "merging" channels.

Below is a modified trainQuoter function returning a channel. 

        func twainQuoter() <-chan string {
                out := make(chan string)
                go func() {
                        for {
                            out <- twainQuotes[rand.Intn(len(twainQuotes))]
                            waitTime := (time.Duration(100 * rand.Intn(10)) * time.Millisecond)
                            time.Sleep(waitTime)
                        }
                }()
                return out
        }

        func main() {
                q1 := twainQuoter()
                q2 := twainQuoter()

                for n := range merge(q1, q2) {
                        fmt.Println(n)
                }
        }

Notice that we pass the two channels to the new "merge" function. The merge function is itself
returning a channel with unified data that we can range over and print. Below is a merge function
modified from the aforementioned pipelines article.

        func merge(qs ...<-chan string) <-chan string {
            out := make(chan string)

            output := func(c <-chan string) {
                for n := range c {
                    out <- n
                }
            }

            for _, c := range qs {
                go output(c)
            }

            return out
        }

Each of these go routines push data back into a shared channel "out". In the example we create an "output" function
which receives a channel from the slice of channels passed into the "merge" function. The example creates a go routine
for each channel in the slice. The go routine ranges over the channel and pipes its output to the shared channel "out".
Finally, "out" is returned and is later ranged over in the main routine to output the merged data.

#### Neat, but is there more?

Why yes there is. Albeit, both of our examples are a little contrived and do not take into real-world channel
conditions. There are problems with potential memory leaks with go routines if an upstream channel stops sending
data. So for next week we will being to explore the fan-out, pipe-lining, and how to safely exit routines that
are done receiving data.
