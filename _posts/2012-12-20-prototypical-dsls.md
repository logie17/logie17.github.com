---
layout: post
title: "Prototypical DSLs"
description: ""
category: programming
tags: [Perl]
---
{% include JB/setup %}
During this time of advent I've been enjoying the daily [OX](http://iinteractive.github.com/OX/advent/) tutorials. One of the interesting aspects of OX is that
it demonstrates the power of [prototypes](http://perldoc.perl.org/perlsub.html#Prototypes) in constructing simple [DSLs.](http://en.wikipedia.org/wiki/Domain-specific_language) [OX](https://metacpan.org/module/OX) comes equipped with the following syntax:

    router as {
        route '/'   => 'root.index';
        ...
    };

Here we see that OX introduces two new keywords "router" and "as". This is a pretty keen trick that is built into Perl with
the power of subroutine prototypes.

## It's actually really easy to do

Just to give an example of how this works let's say we were building our own DSL:

    create foo { 'foo does bar' };
    create baz { 'no idea what baz does' };

This is implemented in the following fashion:

    sub create {
        my $sub_ref = $_[0];
        ...
        $sub_ref->();
    }

    sub foo(&){$_[0]}
    sub baz(&){$_[0]}

The magic occurs at compile time with subroutines foo and baz. The (&) construct is telling Perl to expect a code ref and since it's a prototype we can
exclude the "sub" keyword. This simple fact alone, allows us to chain functions to create what appears to be new keywords, ie create, foo, baz. We then
just pass back the code ref which in turn will be passed into "create" to do with it pleases.

## RTFM

As we an see prototypes give the Perl syntax flexibility when it comes to creating new and interesting DSLs. For more information I would encourage
taking a look at the [perlsub](http://perldoc.perl.org/perlsub.html) [perldocs.](http://perldoc.perl.org)