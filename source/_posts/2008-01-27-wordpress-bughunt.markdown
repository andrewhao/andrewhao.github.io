---
author: andrewhao
comments: true
date: 2008-01-27 22:48:04
layout: post
slug: wordpress-bughunt
title: Wordpress Bughunt
wordpress_id: 858
categories:
- Andrew 2.0
- Geek
tags:
- computer-science
- fighting-fires
- geekstuff
---

I spent a good four hours today digging through [Wordpress](http://www.wordpress.org) code, trying to get our [Cal Christian Fellowship](http://www.ocf.berkeley.edu/~ccf) Web site back up.

The problem: the page seemed to freeze when loading page content. The request would time out, leaving a half-loaded page sans content. I dug into the theme at first, trying to track down which function call could have barfed. I tracked it into the apply_filters() call in the the post.

Wordpress uses a filtering system to manage content. This lets us add filters over all kinds of content. I realize that the problem was stemming from the [Text Control](http://dev.wp-plugins.org/wiki/TextControl) plugin, where something had barfed!

I disabled the plugin and voila! We were good to go again.

Since we only really used Textile markup on the CCF site, I just re-enabled the default Textile plugin and we were back in business.

I don't know for the life of me what went wrong over the weekend in the Text Control plugin, but I don't have the cajones or the time to dig through server logs, et. al. and find out.

Geeking out like nobody's business,

-Andrew
