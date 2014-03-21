---
layout: post
title: "CPAN Review"
description: ""
category: programming
tags: [CPAN, Perl]
---
{% include JB/setup %}
A series of blog posts in which I attempt to review a random module released today on [CPAN.](https://metacpan.org)

# App::cpanminus

Welcome to the revolution my friend, a revolution in package installation simplicity. Today a minor bug fix for [App::cpanminux](https://metacpan.org/release/App-cpanminus) was released. The following has changed the way people have viewed CPAN package installation.

    cpanm Module

Gone are the days of managing and installing modules with the CPAN client. Further it takes advantage of [local::lib](https://metacpan.org/module/local::lib) by default if you don't have permission to install the module in the standard system location.

Here are some of my favorite cpanm commands:

    cpanm -l ~/local-lib/project_name Module

The above will install the module to your local::lib location. It's extremely easy now to have a local::lib for each project and cpanm will honor where you want to place your modules for such a project.

    cpanm . -l ~/local-lib/project_name

Then this command will install all of a packages dependencies provided in a Makefile.PL for example. It comes with some other pretty nifty features like "what dependencies will be installed if I install this module?"

    cpanm --scandeps Module

Finally, it's super simple to install locally:

    mkdir $HOME/bin
    cd ~/bin
    curl -LO http://xrl.us/cpanm
    chmod +x cpanm

BOOM, it's installed. You just need to add bin to your PATH and you're on our way usurping the system and whatever outdated cruft it has installed. Joy to the world, cpanm has come! It's been a valuable tool in my Perl toolkit and hopefully it will be yours as well!
