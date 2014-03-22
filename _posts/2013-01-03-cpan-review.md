---
layout: post
title: "CPAN Review"
description: ""
category: programming
tags: [CPAN, Perl]
---
{% include JB/setup %}
As part of a series of blog posts, I'm going to attempt to find a newly released CPAN module every day and review it.

## Try::Tiny

Today a new version of [Try::Tiny](https://metacpan.org/module/Try::Tiny) was released that fixed some documentation issues. Most people will remember the old school way or maybe
not so old school way of trying to catch exceptions in Perl:

    eval {
        ..some code..
        exception gets thrown
    }

    if ( $@ ) {
        do some clean up and pray $@ hasn't been wiped.
    }

The problem with the above is that the special $@ is a global variable and tends to have problems in older versions of Perl, especially
prior to 5.16. One such problem is that $@ can be wiped if another eval is called, this can often happen if an object has a destructor
that calls eval. So to mitigate this issue developers would sometimes do things such as the following:

    eval { ..some code.. {die} 1; } || do { handle exception }.

Other solutions, probably a better one, would look like this:

    my $error = do {
        local $@;
        eval { .. };
        $@;
    };

    die $error;

So Try::Tiny through the power of prototypes creates a new syntax which does the localizing of $@ for a developer with a cleaner syntax:

    try {
        die "for some reason";
    } catch {
        say "Oh no, I got this error! $_";
    };

This is a much more elegant interface and they even throw in a finally keyword too. Overall this seems nicer but introduces it's own caveats
which a developer using this module need to care when using. The primary one is that Try::Tiny is not adding any new keywords but is using prototypes that create the
illusion of new syntax. So if someone were to forget to put a semi-colon after the try parenthesis they may be unpleasantly surprised that code
inside a catch might be executed prior to a try being executed, for example:

    try { } catch { run_this() }

run_this will be executed before the try block.

Overall I still like Try::Tiny and believe it's a step in the right direction - even if it feels a bit gimmicky. I would encourage most people to use it over eval, but it should be used with care.