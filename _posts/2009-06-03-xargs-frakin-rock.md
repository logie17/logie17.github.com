---
layout: post
title: "Xargs Frakin Rocks"
description: ""
category: 
tags: []
---
{% include JB/setup %}
I'm not a unix sys admin, but I remember the day I discovered xargs and the sheer power I felt. Xargs gave me the ability to run any command on any piped list of files.

For example:

    ls filenames

This would output your list of files. Now lets say you want to cat each file name. With xargs this is as simple as the following:

    ls filenames | xargs cat

###Advanced Usage

Now lets say you have a more complex command, such as a command with arguments. For example lets say you want to take a list of filenames and use perl to search and replace tokens within the files.

    ls filenames | xargs -I {} perl -pi -e 's/foo/bar/' {}

Essentially the I flag tells xargs to use a replace string, that string being the opening/closing curly braces. Whenever this string is used in the command the filename will appear here. Really the command being executed over and over by xargs is
    perl -pi -e 's/foo/bar/ filename

With great power comes a great responsbility, go forth and do good.