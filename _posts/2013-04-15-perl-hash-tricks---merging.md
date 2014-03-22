---
layout: post
title: "Perl Hash Tricks   Merging"
description: ""
category: programming
tags: [Perl]
---
{% include JB/setup %}
Let's say you have two hashes:

    my %first = (
      'one' => 'two',
    );

    my %second = (
      'three' => 'four',
    );

How could we merge the first into the second, so it looked like the following?

    %second = (
      'one' => 'two',
      'three' => 'four',
    );

One thought would be to loop over the first and manually add the key into the second:

    while (($key, $value) = each %first) {
      unless ( exists $second{$key} ) {
        $second{$key} = $value;
      }
    }

This will work, however there is a simpler and more elegant [Perlish](http://en.wiktionary.org/wiki/Perlish) way to do this.

    @second{keys %first} = values %first;

The beauty is that Perl guarantees that values will return in the same order as keys, see "perldoc -f keys".