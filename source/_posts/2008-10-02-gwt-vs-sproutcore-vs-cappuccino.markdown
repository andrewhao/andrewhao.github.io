---
author: andrewhao
comments: true
date: 2008-10-02 00:55:10
layout: post
slug: gwt-vs-sproutcore-vs-cappuccino
title: GWT vs. SproutCore vs. Cappuccino
wordpress_id: 902
categories:
- Geek
- Javascript
- Web
tags:
- cappuccino
- code
- dojo
- framework
- Geek
- gwt
- Javascript
- programming
- sproutcore
- web application
---

I'm looking to develop a Web application with a full-stack Javascript framework like [GWT](http://code.google.com/webtoolkit/), [SproutCore](http://www.sproutcore.com) or [Cappuccino](http://www.cappuccino.org). I'm making the decision to go with a Javascript framework over a traditional full-stack framework (like Rails) because:



	
  * I get richer, desktop-like functionality in the browser.

	
  * I can offload much of the work from the server onto the client.

	
  * There's less Javascript hand-coding so I don't reinvent the wheel.

	
  * I can bring the desktop app programming paradigm straight to the client.


I've been taking a look at [GWT](http://code.google.com/webtoolkit/) (Google Web Toolkit), [SproutCore](http://www.sproutcore.com) and [Cappuccino](http://www.cappuccino.org), and here's some of my observations so far:



	
  * GWT is the most mature framework out of the three at 2 years. SproutCore and Cappuccino have emerged only in the last three months.

	
  * SproutCore is really, really, beautiful. Just look at MobileMe. I'm sure Cappuccino looks much the same. It's probably because both emulate Cocoa, and so take on the same icon set, window chrome, pane behaviors, and so on.

	
  * Objective-J sounds cool. I picked up Cocoa over the summer, and to have it on a Web browser is going to be phenomenal.

	
  * Cappuccino is so new that there aren't that many apps out there (asides from the storied [280 Slides app](http://280slides.com/)). I'm worried that it could be buggy and docs are too sparse.

	
  * Something about Java in GWT makes me cringe. Maybe it's just because I'm not used to that extra compilation step. Maybe it's because I've been mucking in dynamic languages for too long of a time.

	
  * I'm a big sucker for looks. GWT looks pretty frumpy. Honestly, it doesn't get me excited like SproutCore applications do in all their Aqua-like goodness.

	
  * The Ext widgets from [Ext-GWT](http://extjs.com/products/gxt/) are only slightly better. I guess I shouldn't complain... but gosh, I really like OS X-looking widgets. What can I say, I was brainwashed :)

	
  * SproutCore and Cappuccino are slower: they simply can't beat the compiler optimizations of compiled and optimized JS in GWT. Then again... we're going to have crazy fast JS engines in the next year or so.

	
  * I considered [Dojo](http://dojotoolkit.org/). Sort of. I like it because it's more mature than SC, but it's slower than GWT and doesn't have as many widgets as Ext.


Honestly, even though my "ooh-shiny" sense wants to use SproutCore, my objective reasoning tells me to go for GWT for its stronger support, more maturity, better performance and a fuller set of widgets. Anybody else want to weigh in?
