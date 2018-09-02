---
author: andrewhao
comments: true
date: 2012-06-20 09:28:13
layout: post
slug: updating-max-file-limit-on-osx-lion
title: Updating max file limit on OSX Lion
wordpress_id: 1159
categories:
- Andrew 2.0
- Apple
- OS X
- Tips
tags:
- code
- files
- launchctl
- osx
- shell
- ulimit
---

I've been hitting a lot of "Maximum file limit exceeded" dialogs after a long day at work -- at any point in time I've got a kajillion Chrome tabs open, five or six Rails envs running (for dev and test) + Guard/Spork actively watching tests, and Sublime with another kajillion tabs open.

Turns out that OSX limits the number of open file descriptors per process to 256. Time to bump up the limit:

First, check out your current file limit:

`$ launchctl limit`

    
        cpu         unlimited      unlimited      
        filesize    unlimited      unlimited      
        data        unlimited      unlimited      
        stack       8388608        67104768       
        core        0              unlimited      
        rss         unlimited      unlimited      
        memlock     unlimited      unlimited      
        maxproc     709            1064           
        maxfiles    256            unlimited


Okay, let's crank 'er up. First let's create `/etc/launchctl.conf`

`$ sudo touch /etc/launchctl.conf`

And let's open it with your editor of choice. Add the following line to the new file:

`limit maxfiles 16384 32768`

Restart your computer. Boom. Easy.
