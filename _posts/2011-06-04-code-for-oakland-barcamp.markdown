---
author: andrewhao
comments: true
date: 2011-06-04 18:43:19
layout: post
slug: code-for-oakland-barcamp
title: Code For Oakland Barcamp
wordpress_id: 1058
categories:
- Andrew 2.0
---

[caption id="" align="alignnone" width="500" caption="Is that Jon Chan I see? Yes it is. Photo credit Oakland Local."][![](http://farm4.static.flickr.com/3527/5804304389_e539db36a2.jpg)](http://www.flickr.com/photos/oaklandlocal/5804304389/)[/caption]

A few notes from this [one-day barcamp/hackfest](http://codeforoakland.org/). The goal was to create mobile apps for Oakland, with a special emphasis on serving underserved populations.



	
  * Text messaging is still king. While smartphones and their apps are at the leading edge and grabbing all the attention these days, most underserved populations don't have access to these services. Lots of apps today are using [Twilio](http://www.twilio.com/) or [Tropo](http://www.tropo.com) APIs as their SMS/voice gateways.

	
  * Open data is awesome. I learned about [ScraperWiki](http://www.scraperwiki.com), which basically puts site scraping code into the cloud so people can maintain your scraper script long after you've tossed it. Some staff from maplight.org are here.

	
  * Open data is also difficult to maintain. One presenter mentioned how Oakland keeps a database of social services--that's great, but what if the organization shuts down and/or changes its hours? Who's responsible for updating the information? IIRC, the estimate was that 30% of the listings kept by the City are no longer valid. There need to be active efforts to combat dataset decay.

	
  * By the way, here's a [list of Oakland GIS datasets](http://codeforoakland.org/data-sets/). Some of it really sucks (filled with garbage spam data). Some of it is useful.




#### Today's Projects:





	
  * [comtxt](http://comtxt.nodester.com/): text gateway for community organizers. [github.com/ryanjarvinen/comtxt](http://github.com/ryanjarvinen/comtxt)

	
  * freexchange.org. mobile interface to donated goods.

	
  * oakland:pm. get oakland high-schoolers connected to afterschool programs. [github.com/jedp/oakland.pm](http://github.com/jedp/oakland.pm)

	
  * txt2wrk: text-based job matching. this one is unique because it is a complete SMS/voice based interface. connects to craigslist. call it and it reads back craigslist job postings. targeted for parolees.

	
  * Oakland Food Finder: helping people find healthy, locally-grown food.

	
  * BettaSTOP: help people find buses, access bus schedules. in oakland, many underserved communities depend on making and finding the right bus. it also allows users to give feedback on buses, remark on their timeliness, and talk about bus route features. Live & in production: http://www.bettastop.net.




#### What I did


I'm helping out the Oakland:PM team, which is in the process of building out a service to get high schoolers connected to city-funded afterschool programs. The idea is that they can pull up their mobile phones and see what's available to them while they're kicking it with friends and bored out of their minds.

While the others hacked on wireframes and some code, I worked on a few user stories and resolved to interview a few of my contacts who work for the YMCA in East Oakland. We won a $500 grant from the City to see this thing through in July. Here's hoping that we'll make it.

So far the app is a [nodejs](http://nodejs.org) app with pages served with the [Express](http://expressjs.com/) framework. We threw around ideas of using [Sencha Touch](http://www.sencha.com/products/touch/), but I think that decision is out of my hands. We'll see how we proceed.

Source code can be found at: [http://www.github.com/jedp/oakland.pm](http://github.com/jedp/oakland.pm)
