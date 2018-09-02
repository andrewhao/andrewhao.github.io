---
author: andrewhao
comments: true
date: 2011-02-01 18:40:40
layout: post
slug: the-making-of-sos-intro
title: 'The making of SOS: Intro'
wordpress_id: 1012
categories:
- Andrew 2.0
- Art
- Geek
- God
- Software
- Web
---

[![Save Our Souls - Logo](http://farm6.static.flickr.com/5095/5408231821_b4d0fd6837.jpg)](http://www.flickr.com/photos/andrewhao/5408231821/)

I'm starting a project for my church's art show that integrates Twitter, print design and light. I'm titling it "Save Our Souls".

The theme of the art show is "Instead of Ashes", a reference from of [Isaiah 61](http://www.esvonline.org/search/isaiah+61/) which highlights of God's promises of redemption for the world's suffering through Jesus.


## What does it do?


Essentially, what I want it to do is parse the global Twitter firehose and display a live stream of tweets that highlight brokenness and pain: tweets about the world's injustices, breakups, deaths, disappointments, and even ennui. This is inspired by [twistori](http://twistori.com).

At the same time these tweets are floating across the screen, I want to illuminate a part of the printed verse in physical space that corresponds to the tweet.

Imagine:

Tweet: _I just want everyone to understand something......I'm SEVERELY depressed right now......my (ex)girlfriend is one of the greatest things to happen to me and she's gone._

At the same time, the LED array behind "he has sent me to bind up the brokenhearted" from Isaiah 61:1 would light up and pulse.

And so on, for each tweet that flies across the screen.


## Architecture
[![Isaiah 61 Architecture](http://farm6.static.flickr.com/5020/5408232737_0ba8545d4e.jpg)](http://www.flickr.com/photos/andrewhao/5408232737/)


node.js would process messages from the Twitter streaming API keyed off of certain keywords and filter them by (negative) sentiment, and pushed onto queues. Queues would be drained in a fair manner and pushed out to a browser frontend, while a socket connection to an Arduino script would be responsible for pulsing the LED array.

Of course, there's a lot of questions left unanswered here: How do you make an LED array? How do you do (reliable) sentiment analysis? How big should the print graphic be? How far behind the print should the LED arrays be placed for the light to be sufficiently diffuse, yet bright enough to be visible? I'm not sure yet.


## First steps


So I just bought an Arduino and a ton of LEDs off of eBay. I'm trying to relearn all the EE40 I tried so hard to forget way back in my undergrad days.

[caption id="" align="alignnone" width="500" caption="Arduinos are cool. I had to do my fair share of head-scratching to figure out resistor values."][![Arduino is hooked up](http://farm6.static.flickr.com/5096/5408162241_201f757ddd.jpg)](http://www.flickr.com/photos/andrewhao/5408162241/)[/caption]

[caption id="" align="alignnone" width="500" caption="A conceptual design for the print graphic."][![Print Concepts](http://farm6.static.flickr.com/5098/5408870750_9fa4951f0d.jpg)](http://www.flickr.com/photos/andrewhao/5408870750/)[/caption]

[caption id="" align="alignnone" width="500" caption="Basic node.js backend with a rudimentary frontend streams live tweets."][![Testing backend.](http://farm6.static.flickr.com/5137/5408765510_3381c46945.jpg)](http://www.flickr.com/photos/andrewhao/5408765510/)[/caption]

See the (rudimentary) nodejs/client code at [github](https://github.com/andrewhao/isaiah-61-project).

See the [rest of the photos](http://www.flickr.com/photos/andrewhao/sets/72157625956085386/with/5408765510/).

More to come!
