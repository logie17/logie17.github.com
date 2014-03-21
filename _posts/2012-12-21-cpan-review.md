---
layout: post
title: "CPAN Review"
description: ""
category: programming
tags: [CPAN, Perl]
---
{% include JB/setup %}
As part of a series of blog posts, I'm going to attempt to find a newly released CPAN module every day and review it.

## Test::Pretty

Today on cpan a new version of [Test::Pretty](https://metacpan.org/module/Test::Pretty) was released. This latest version of Test::Pretty, v0.22, looks to be a simple bug fix, however it's the first time I've encountered it. Essentially it looks like a way to print out a prettier output that Test::More produces.

A simple example:

    use strict;
    use warnings;
    use Test::Pretty;
    use Test::More;

    is 1,1, "Of course 1 is 1";
    ok 1, "Yes is true in Perl, weird I know";

    is 1, 0, "Duh, 1 doesn't equal 0";

    done_testing;

Now if we run this we get the following output:

![Test::Prety](/images/posting/test_pretty_output.png)

## Check it out

So I would reocmmend checking it out and prettify your test outputs.