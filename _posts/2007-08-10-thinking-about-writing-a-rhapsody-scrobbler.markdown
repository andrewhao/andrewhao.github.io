---
author: andrewhao
comments: true
date: 2007-08-10 12:10:11
layout: post
slug: thinking-about-writing-a-rhapsody-scrobbler
title: Thinking about writing a Rhapsody Scrobbler
wordpress_id: 777
categories:
- Andrew 2.0
---

_Filed under "Fun Ideas That Could Take Three Hours or Three Months"_

I want to write a [Rhapsody](http://www.rhapsody.com) [scrobbler](http://www.last.fm/help/). It's already been done as a [desktop application](http://www.atlansky.com/dev/rhapsodyscrobbler.html), but I don't want to boot up another application while I'm playing music. I want to do it as a hosted Web service.

Does anybody want to look into this? I'm thinking the design would be simple: regularly consume the Rhapsody user's [Tracks RSS feed](http://www.rhapsody.com/myrhapsody/feeds.html) and plug it into AudioScrobbler via its [API](http://www.audioscrobbler.net/data/webservices/).
