---
layout: post
title: "new post"
description: ""
category: 
tags: []
---
{% include JB/setup %}
### Safe Map With Channels

In my previous blog post I described how a programmer could build a concurrently
safe map using Go's sync library. For all intents and purposes using sync
is a perfectly fine pattern to follow, further, for routine usage it's probably the
best solution. However, Go offers another way to do this which some may dispute is more
idiomatic.

#### Is the solution channels?

The other way involves a first class type in Go called "channels". Channels are best described
as a conduit in which data can be sent and received. In many ways channels are akin to queues
which can assist when it comes to synchronization. Channels give a programmer a contrivance
for synchronicity across go routines.

#### Channels allow synchronization

As we mentioned in the previous blog article, shared memory in a concurrent environment
can prove to be problematic.  One way we could solve this problem of is to use channels.
For example, a programmer could have a go routine that runs in parallel and listens for
updates on the receiving end of a channel.  Since the information flowing through the channel
are guaranteed to be synchronized a map can be safely updated.

Below is a very simple example of channels in conjunction with a go routine.

```
       m := make(chan int)
        go func() {
                for num := range m {
                        fmt.Println(num)

                }

        }()
        m <- 1
        m <- 2
        m <- 3

```

The above, albeit trite, piece of code will always print in order of what is put on the channel. The output
will be:

```
        1
        2
        3

```

So let's extend this idea to make a safe map. The key to implementing is to spin up a parallel
go routine that will listen to updates on a channel and safely perform operations on a map. Once again,
this is safe because channels guarantee synchronization.

Let's take a look at the following code example:

```
        type safeMap chan commandData
        func NewSafeMap() safeMap {
           m := make(safeMap)
           go m.run()
           return m
        }

        func (m safeMap) run() {
            store := make(map[string]interface{})

            for command := range m {
                switch command.action {
                    case 1:
                        store[command.key] = command.value
                }

            }
        }
```

From the example above we create a safeMap type, which is just a channel. Next, we
build a constructor that does two things: 1) it initializes the channel and 2) calls the method "run"
in parallel with a go routine. The run method is akin to our previous example, it will listen to
whatever data comes through the channel and using the "switch" keyword respond to specific actions.
In our example we have one action which does an insert or saves a key/value into the map "store".

#### What is the commandData?

Take a look at the next example. The command coming through the channel is defined as a commandData struct.
This is just a payload of information to tell the run function what to do. This payload contains an
"action", "key", and a "value".  The action is used in the above switch statement to signal what the 
run method should do to the map, the key and value is what is used for the map data.  Right now we have one
supported action, insert, which is designated by the numeric "1".  So every time the insert
method is called it pushes data into a channel. The other side of the channel is listening
within the "run" go routine that will save the data into a key/value map.

```
        type commandData struct {
            action int
            key string
            value interface{}
        }

        func (m safeMap) Insert(key string, value interface{}) {
            m <- commandData{action: 1, key: key, value : value }
        }

```

#### Are there other actions?

The next obvious action we could add is delete. Let's create a new method called
"delete" that will accept a key and delete the value.

```
        func (m safeMap) Delete(key string) interface{} {
                m <- commandData{action: 2, key: key }
        }

```

Next we will need to modify the "run" method to support the new delete action:

```
    func (m safeMap) run() {
        store := make(map[string]interface{})

        for command := range m {
            switch command.action {
                case 1:
                    store[command.key] = command.value
                case 2:
                    delete(store, command.key)
            }

        }
    }

```

So now all together, we can call our new safeMap the following ways:

```
        m := NewSafeMap()

        m.Insert("foo","bar")

        m.Delete("foo")

```

#### Where do we go now?

In the next blog post I would like to demonstrate how we can receive data
back from a go routine that is running in parallel. This sort of pattern
will prove useful if we want to retrieve value from our safeMap.
