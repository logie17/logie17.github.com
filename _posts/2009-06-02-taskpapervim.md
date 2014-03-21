---
layout: post
title: "Taskpaper.vim"
description: ""
category: 
tags: []
---
{% include JB/setup %}
I don't know about you but my work day consists of working with random bits of paper with lists on them. As things come up I write it down on my scrap piece of paper and as I complete items I scratch it off the list. In this day of technology I could never find an electronic equivalent to keep track of all my lists. In some of my downtime I browse through vim plugins and I came across taskpaper.vim. This plugin replicates the functionality of the Mac application [TaskPaper](http://www.hogbaysoftware.com/products/taskpaper).
###Keep it simple
Tasks are simple, and should be kept simple. The reason I've never been able to find an alternative to pen and paper is simply because most electronic alternatives are simply too complex. Taskpaper.vim has 4 simple mappings and adding a new task is as simple as firing up vim. Here is an example of taskpaper format:

    Todays Tasks:
    - Fix bug with search: @bugs
    - Migrate the code for project X: @projectx
    - Talk with Ted about my review: @done

    The Special Project:
    - Develop UI: @done
    - QA: @todo
    - Deploy: @todo

As you can see this file is relatively simple and easy to read. Projects are distinguished with text followed by the colon, ":", and tasks begin with a dash -. The @, at sign, allow for tags on each task. There can be multiple tags per task.

The new mappings which I mentioned are the following: *\td*, *\tc*, *\tp*, and *\ta*. *\td* marks a task as done, this will add the tag @done to your task. *\tc* will show all tasks that are have the same tag. *\tp* will fold all projects and *\ta* will show all projects and tasks.

Finally I created some shortcuts in my .vimrc file to make things a little simpler.

    $tasks = $HOME . '/tasks/logans_tasks.taskpaper'
    nmap <C-t> :tabnew $tasks<CR>

Now whenever I press *CTRL-T* a new tab will open with my taskpaper tasks!

It simply is cool.
