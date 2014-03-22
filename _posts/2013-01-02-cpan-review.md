---
layout: post
title: "CPAN Review"
description: ""
category: programming
tags: [CPAN, Perl]
---
{% include JB/setup %}
Recent CPAN Module Review - Moox::ClassAttribute|2013-01-02|# Recent CPAN Module Review

As part of a series of blog posts, I'm going to attempt to find a newly released CPAN module every day and review it.
Because of the end of the year holidays I eneded up taking a bit of time off from blogging, so I'm restarting this series
and will hopefully try to keep up on a daily schedule.

## Moox::ClassAttribute

There are times when you may need an attribute on a Moo object to be accessible at the Class level. This could easily be done with Moose
using the module [MooseX::ClassAttribute,](https://metacpan.org/module/MooseX::ClassAttribute) however, not so easy with Moo, until today. To start the new year off [Moox::ClassAttribute](https://metacpan.org/module/MooX::ClassAttribute) was released.
With the following code you can now have class level attributes:

    package MyClass;

    class_has class_level => (
        is => 'rw',
        default => 'foobar'
    );


    print MyClass->class_level;

This module was inspired by MooseX::ClassAttribute, which obviously requires Moose, but now the same power is available for Moo.
Outside the obvious issue that class level attributes tend to be another form of global variables. Moo::ClassAttribute,
offers some flexibility that can't be achieved with a instance level attribute.  However, as stated in the documentation,
it currently does not support subclassing which may or may not be a big problem.  This module was just released, so it'll be interesting to see how
it matures and maybe one day it'll be as feature complete as MooseX::ClassAttribute.

So there you have it, if you need globals at the class level it's now available with Moo.