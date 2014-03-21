---
layout: post
title: "CPAN Review"
description: ""
category: programming
tags: [CPAN, Perl]
---
{% include JB/setup %}
A series of blog posts in which I attempt to review a random module released today on [CPAN.](https://metacpan.org)

## DBI

After over twenty years [DBI](https://metacpan.org/module/DBI) is still releasing new versions. Today a new version of DBI, [1.622_931](https://metacpan.org/module/TIMB/DBI-1.622_931/Changes#Changes-in-DBI-1.623-svn-r15545-21st-Dec-2012), emerged on CPAN that appears to fix a number of bugs, deprecates old code, and offers some documentation improvements. I still remember when I was learning [Perl](http://www.perl.org), I had the "Learning Perl" book in my left hand and the "MySQL & mSQL" book in the other. Within the latter there was a chapter dedicated to DBI and how to write Perl code to interface with M\[y\]SQL database. The pages would outline steps to setting up your database handler; using the all so familiar prepare, execute, and do method calls.

The all too familiar code:


    my $sth = $dbh->prepare("select foo from bar where bar = ?");
    $sth->execute( $value );

    while ( my $row = $sth->fetchrow_hashref ) {
        ...
    }

## Workhorse

Even today I find myself writing ad-hoc DBI scripts to do bit munging in a database. In fact, I'm still maintaining legacy code that makes direct calls to the database via DBI. Yes we may have nicer tools and [ORMs](http://en.wikipedia.org/wiki/Object-relational_mapping) now, but there is nothing like grabbing a handler by the horns and sending a "do" method call with raw SQL straight to the database.

So I salute you [Tim Bunce](https://metacpan.org/author/TIMB) and I salute all of the developers who have over the years kept the project alive and fresh. DBI has been my bread and butter for most of my Perl career and I'm fairly certain I'll be writing prepares, dos, and executes for a while into the future.
