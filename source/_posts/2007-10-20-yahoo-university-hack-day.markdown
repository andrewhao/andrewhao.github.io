---
author: andrewhao
comments: true
date: 2007-10-20 02:20:08
layout: post
slug: yahoo-university-hack-day
title: Yahoo! University Hack Day
wordpress_id: 810
categories:
- Andrew 2.0
- Geek
- Interfaces
---

So Hsiu-fan and I had floated the idea of doing a hack for [Yahoo! University Hack Day](http://developer.yahoo.com/hacku/). We had even polled some of our friends about it, and the consensus seemed to be "We'll see what we can do with the time we're given."

Well it turned out we didn't have very much time. Today I opened up my email inbox to discover that Hack Day was today, and the actual time to hack was only from 2PM to 6PM.

So I head on over to the Woz with the intention of:



	
  1. Eating free food

	
  2. Grabbing a t-shirt or two

	
  3. Talking to some of the Yahoo! devs

	
  4. Giving a couple of hours as a freelance developer.


While I easily managed to achieve #1, 2 and 3, #4 (joining a random group) was something that I didn't expect to do.

Hsiu-Fan showed up half-way through the competition and told me "Okay, I have 50 minutes. What do you want to do?" We had floated the idea of doing the [Rhapsody scrobbler](http://blog.g9labs.com/2007/08/10/thinking-about-writing-a-rhapsody-scrobbler/). "Okay," he tells me. "I think I can do that in 50 minutes."

But by then, I've been snagged by this other group, composed of folks mainly from the [CS Undergrad Association](http://csua.berkeley.edu). The idea? The creation of a **commuter Web application** that would take peoples' commute details and sort them into carpools based on convenience.

The problem? These guys were serious Java heads. I was the lone Web developer. We were going to have problems. Fortunately we managed to hash out a somewhat working hack using the skill sets that we had.

Okay, it was a "hack" in every sense of the word. The other guys didn't want to implement a database (which means no locking, no synchronization). I was parsing browser inputs through PHP and writing them out to a flat file. Then something got slurped by our Java backend and requests were shot back and forth between Yahoo! Maps and our service. Then the resulting routes were written out to a flat file and a PHP script consumed it on demand, displaying calculated routes.

My job? I was the frontend guy, interface guy, PHP guy, and client-side guy. I put together a pretty quick-and-dirty interface (see below) using [YUI grids and reset](http://developer.yahoo.com/yui/grids/). Then I did a pretty massive Frankenstein CSS job, piecing bits of code from the [Blueprint CSS framework](http://code.google.com/p/blueprintcss/), previous projects and [Wejoinin](http://www.wejoinin.com).

(Aside: If there's one thing I appreciate about a CSS framework, it's the flexibility and speed it affords you. I love, particularly with Blueprint (which I would have preferred to use on this project but didn't because it _is_ a Yahoo! Hack Day), the ability to lay out a grid-based layout using CSS selectors in a fraction of the time it takes you to build it again from scratch.)

Then I took a deep breath and summoned up enough of the PHP left in the recesses of my brain.

Oh yeah, Hsiu-Fan didn't manage to finish the Scrobbler on time (50 minutes was pretty optimistic), but we're looking into finishing it up sometime, somewhere else. And it will be cool and we will feel fuzzy.


#### Hack day lessons





	
  * Come in with a cool idea with a cool team ready to execute. Don't assemble something together last minute.

	
  * Yahoo! cares more about people that are passionate than  about people who  have impressive resumes.

	
  * Your project idea should be limited enough in scope to be polished.

	
  * Even if your team strengths don't align well, you'll always find a way to do [your hack].

	
  * Come on, Yahoo!, could we get a little more than 4 hours to finish? :)


All in all, a bunch of fun. I'd definitely do it again. And I'd pray that I'd have a little more time to execute. And hope that people don't give me any more weird looks when I tell them I was at a "programming contest."

[![Commuter Interface](http://blog.g9labs.com/wp-content/uploads/2007/10/commuter_screenshot_2.thumbnail.png)](http://blog.g9labs.com/wp-content/uploads/2007/10/commuter_screenshot_2.png)[![Commuter Interface](http://blog.g9labs.com/wp-content/uploads/2007/10/commuter_screenshot_1.thumbnail.png)](http://blog.g9labs.com/wp-content/uploads/2007/10/commuter_screenshot_1.png)

-- Edit:

[![](http://farm3.static.flickr.com/2368/1646789668_342a79679b_m.jpg)](http://flickr.com/photos/rckenned/1646789668/in/set-72157602538180791/)


> Berkeley Hack Day happened Friday at a frenetic pace.  We invaded the Woz in Soda Hall and students were trickling in and out all day with a core group of four hack teams staying for the duration and a couple of teams working remotely.  The teams fought Java, Python, C++ and PHP all day long, with Java defeating at least a couple of team members with its multiple layers of input streams required just to pull in a simple text file.  We also fought with RSS feeds from Craigslist, news archives and local Berkeley event listings.

In the end it was a very close race between two hacks that rose above the rest: an extremely useful carpool commuter application that matched up peoples' addresses and their daily destinations and tried to organize the most efficient combination of carpools, and a magnificent hack that provided a cell phone interface to the old game of Twenty Questions.  The impressive mix of technologies required to answer the call automatically, acquire the data for the twenty questions, do the text-to-speech translation and issue the FFT code to decipher the user responses from the cell phone won over the judges to take the top prize.


[Berkeley University Hack Day Wrap-Up Yahoo Developer Network blog](http://developer.yahoo.net/blog/archives/2007/10/berkeley_univer.html)
