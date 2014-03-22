---
layout: post
title: "Uniquify an array because it's fun"
description: ""
category: programming
tags: [Perl]
---
{% include JB/setup %}
[Perl](http://www.perl.org) has some built ways to take an array of random elements and uniqify those elements. Or to put it in another terms
let's say we have a list of elements and we want to filter out all duplicates. Below is an example of a routine that might
do this very thing:

    sub uniquifier {
        my %uniq = map { $_ => 1 } @_;
        return keys %uniq;
    }


By nature key value hashes can only have one key element in a set, so using Perl's built in ["keys"](http://perldoc.perl.org/functions/keys.html) function we can get a unique list.
The only problem with this solution is that keys function will return the keys in random order
and we lose the orderliness of our array. We can fix this by iterating over the list in order
and only returning what has been seen:

    sub uniquifier {
        my %seen;
        return grep { $_ } map { $seen{$_} ? undef : do { $seen{$_} = 1; $_ } } @_;
    }

What we have here is that we're iterating over the passed in array in order and returning only elements
that have been stored in the %seen hash. However, this could even be simplified in the following way:

    sub uniqifier {
        return grep { !$seen{$_}++ } @_;
    }

Thanks to how [++](http://perldoc.perl.org/perlop.html#Auto-increment-and-Auto-decrement) works this magical operator does the following according to the [perlop](http://perldoc.perl.org/perlop.html) perldoc:

    "undef" is always treated as numeric, and in particular is changed to 0
     before incrementing (so that a post-increment of an undef value will
     return 0 rather than "undef").

So what happened is that the first item through the grep is coerced to be 0 as a value; thanks to
the how ++ operates. 0 is returned from this evaluation and that makes !0 to be true and thus returned from grep.
The next time the same element is encountered !$seen{$_}++ will return undef and pass over it. This is a very elegant
way of using grep and a hash cache to return only unique elements in order.  Further, this is the same
methodology used in [List::MoreUtils](https://metacpan.org/module/List::MoreUtils) and it's associated "uniq" function.