---
author: andrewhao
comments: true
date: 2010-03-14 13:04:19
layout: post
slug: upgrading-wejoinin-to-unicorn
title: Upgrading Wejoinin to unicorn
wordpress_id: 963
categories:
- Andrew 2.0
tags:
- ruby on rails
- unicorn
- Wejoinin
---

(Reposted from the [Wejoinin Blog](http://blog.wejoinin.com/2010/03/14/server-transition-nuts-and-bolts/))

When you run a Web app like Wejoinin on minimal VPS resources (read: we're too poor to get a beefy server), it forces you to go lean. We started to realize a year ago that our out-of-the-box Rails and nginx/Mongrel setup was starting to show its age; resource utilization would climb every so often and we'd have to kill and restart a Mongrel thread.

Well, with last night's Wejoinin push, we've upgraded our server environment a few ways:



	
  1. We've switched to [prgmr.com with a killer deal](http://prgmr.com/xen/) on a 512MiB VPS. This is a step up from the 256 slice we used to hold at [Slicehost](www.slicehost.com) at almost half the price.

	
  2. We've switched from vanilla Ruby to [Ruby Enterprise Edition ](http://www.rubyenterpriseedition.com/)-- advertised to take "33% less memory [when used with Passenger]". It's got a tweaked garbage collector, memory allocator and the ability to go in a tweak memory usage settings for yourself.

	
  3. We've set up [Unicorn](http://unicorn.bogomips.org/), the new HTTP server on the block. It's special in that each worker is in its own process, meaning that the load balancing is done natively by the OS. Also, this means that should a worker process start to get bloated, we can take it down gracefully without touching the others. Really. [We can trust the OS](http://tomayko.com/writings/unicorn-is-unix). Plus, [Git's doing it](http://github.com/blog/517-unicorn).


With all these tweaks, we should be seeing Wejoinin rarin' to go. Let us know what you think!

-Andrew
