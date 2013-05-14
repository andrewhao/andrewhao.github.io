---
author: andrewhao
comments: true
date: 2011-02-12 17:38:45
layout: post
slug: currently-frustrated
title: 'Currently: frustrated'
wordpress_id: 1027
categories:
- Art
- SOS
---

So I got the poster printed, and the LEDs currently show through the board pretty well. This is good:

[![Printed poster, testing the light](http://farm5.static.flickr.com/4134/5437339409_14b14199d0.jpg)](http://www.flickr.com/photos/andrewhao/5437339409/)

But last night I spent a good chunk of my evening and early morning hitting a lot of walls:



	
  * I might have to throw out the idea of using the Twitter Streaming API. For the kind of searches I need to do, I just can't get enough granularity to use live information. Plus, I can only open one connection to the API at a time, which is not good if I need to run four listeners at a time.

	
  * I couldn't get the [Classifier](http://classifier.rubyforge.org/) Ruby gem to work; which looked like the easiest implementation of an LSI/naive Bayes classifier out there. The next closest thing was [nltk](http://code.google.com/p/nltk/), and there was no way in heck I had the time to figure that out. Plus, I realized that creating a training set was a LOT more work than I thought I had. So... scratch the machine intelligence out of this. I'm just going to manually search for specific search terms.

	
  * New solution: Periodically use the Search API to grab results. This allows more exact search results and gives me the ability to tweak the search terms while the demo loop is running.

	
  * Event-driven programming is throwing me for a loop (ha, get it?). After perusing the [node.js docs](http://nodejs.org/docs/v0.4.0/api/) for the better part of an evening, I think I need to re-architect the code. I need to create a simple event-driven queue, which is confusing because it seems like something simple, yet support isn't built in. node provides so little out of the box.

	
  * It could be more difficult to set up a socket connection to Arduino than I thought. I may have to set up a socket connection in python with [python-firmata](https://github.com/lupeke/python-firmata) to interface with the Arduino. Other Arduino/serial proxies report not working well with Snow Leopard.

	
  * I haven't yet thought about the Web interface.

	
  * I haven't thought about how I'm going to hang the piece. I have some scrap wood and fishing wire, but I haven't thought about whether it's possible to hang all those LED arrays w/o some weird gravity issues.

	
  * Woodwork help? I need to figure out how to use a rotary saw.


So uh, yeah, I'm getting nowhere but at least I know what I still need to do.
