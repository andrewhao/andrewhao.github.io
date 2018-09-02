---
author: andrewhao
comments: true
date: 2011-02-23 10:51:37
layout: post
slug: save-our-souls-a-twitter-art-installation
title: Save our souls - a Twitter art installation
wordpress_id: 1038
categories:
- Andrew 2.0
- Art
- Geek
- God
- SOS
---

Here's how the installation looked on the day of the art show.

[caption id="" align="alignnone" width="333" caption="We mounted the installation on the inside of the Regeneration cafe. The Arduino lies behind the Macbook behind the monitor."][![Installation](http://farm6.static.flickr.com/5180/5467033003_c1c14c5dc0.jpg)](http://www.flickr.com/photos/andrewhao/5467033003/)[/caption]

[caption id="" align="alignnone" width="333" caption="The LEDs are mounted on breadboards suspended on fishing wire, binder clips, rubber bands, chopsticks, and a prayer."][![Mounted LED array](http://farm6.static.flickr.com/5296/5471602272_85203b475e.jpg)](http://www.flickr.com/photos/andrewhao/5471602272/)[/caption]

[caption id="" align="alignnone" width="333" caption="Finished it just in time."][![In action](http://farm6.static.flickr.com/5057/5471598060_f1bb64a83f.jpg)](http://www.flickr.com/photos/andrewhao/5471598060/)[/caption]



[Save our souls - Twitter art installation](http://vimeo.com/20267056) from [Andrew Hao](http://vimeo.com/user807863) on [Vimeo](http://vimeo.com).






> 
What are people saying about the ashes in the world today? This installation visualizes a live Twitter stream on heartache, injustice, loss, and our city and matches them up with the redemptive promises of Isaiah.

Life is difficult, and redemption is something we all long for. What changes do you hope for in your life or in the world? Send a response from your Twitter account to @sos_61 and watch the installation react. If you'd like to be kept anonymous, send your response in a DM to @sos_61.

"I hope for ____"
"I wish that ____"
"I want to see ____"






### A few notes





	
  * Web interface is a fullscreen Google Chrome window. [socket.io](http://socket.io/) is the Websocket interface to the [node.js](http://nodejs.org) backend. The slide transition is animated via a CSS3 animation, and the red overlay is a simple SVG shape plotted with the help of [RaphaëlJS](http://raphaeljs.com).

	
  * The Twitter backend is a collection of four self-updating Twitter searches, one for heartache ("i feel lonely, sad, depressed"), injustice ("violence, war, oppression, justice"), death ("rest in peace, passed away"), and Oakland ("oakland"). A blacklist filters out undesirable tweet keywords ("justin bieber").

	
  * Additionally, the backend connects to Twitter via the [Streaming API](http://dev.twitter.com/pages/streaming_api) and displays a special animation for users who reply via tweet to the [@sos_61](http://www.twitter.com/sos_61) account.

	
  * The installation picks a tweet to display and pulses the LED array corresponding to the right tweet.

	
  * Communication to the Arduino happens via a python script over the Firmata protocol, using the [python-firmata](https://github.com/lupeke/python-firmata) library. The nodejs server signals the script over a socket connection which will run the pulse animation on the correct pin.

	
  * I printed the graphic on an oversize printer with the good folks at [Alameda Copy](http://alamedacopy.com). Friendly service, fast turnaround, very reasonable prices. Ask for Joe.




### Links





	
  * [Source code on github.](http://www.github.com/andrewhao/isaiah-61-project)

	
  * [Flickr photos from the art show.](http://www.flickr.com/photos/andrewhao/sets/72157626107989740/)

	
  * [Photos of the creation process.](http://www.flickr.com/photos/andrewhao/sets/72157625956085386/)

	
  * Other posts on the process: [[1]](http://www.g9labs.com/2011/02/01/the-making-of-sos-intro/), [[2]](http://www.g9labs.com/2011/02/11/update/), [[3]](http://www.g9labs.com/2011/02/12/currently-frustrated/), [[4]](http://www.g9labs.com/2011/02/15/arduino-and-python-firmata/)



