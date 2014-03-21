---
layout: post
title: "What do you declare?"
description: ""
category: 
tags: []
---
{% include JB/setup %}
If you use the bash shell, if you run 'declare -f', what sort of functions do you have? This is the first post in the series *What do you declare*, and I'll post useful bash functions which I've accumulated over the years.

The first and probably the most useful function I have is *mygrep*.

    function mygrep()
    {
        if [ $1 ]
        then
            find . -path '*/.svn' -prune -o -type f -exec grep -nHo "$1" '{}' \n;
        else
            echo "Error: Please supply argument"
        fi
    }
