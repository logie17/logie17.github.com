---
layout: post
title: "Basic hash manipulation"
description: ""
category: programming 
tags: [Perl]
---
{% include JB/setup %}
Do you have find yourself doing a lot of this?

   Class::Foo->something( $arg{'apple'} ? ( apple => $arg{'apple'} : () ));

Well it now can be as simple as:

     Class::Foo->something( slice(\%args, qw(apple)) );

And what if you want to pull out a slice with certain keys you could do:

    my %fruit =	map { $_ => $key_values{$_} } grep { /(apple|bananna)/} keys %key_values;

but it could be replaced it with the following:

    my %fruit = slice_grep { /(apple|bananna)/ } \%key_values;

These two common patterns and other useful functions of hash manipulation is available with [Hash::MoreUtils.](https://metacpan.org/module/Hash::MoreUtils) Take a look!

sqlite> select title,post_date,markup from entries where id= 17;
Object method interpolation|2012-12-19|# Perl tip of the day - Object method interpolation

    print "Some random string and we want the value from $object->say_something";

Will return something like:

   Some random string and we want value from Object=HASH(0x7fb342826788)->say_something

Then you would probably do something like this to make it work:

    my $something_to_say = $object->say_something;
    print "Some random string and we want the value from $something_to_say";

But you could just do this:

    print "Some random string and we want the value from ${\$object->say_something}";

Essentially this creates a string ref which is immediately deref for display. It's a simple, albeit, hacky
way of calling an object method directly within a string.