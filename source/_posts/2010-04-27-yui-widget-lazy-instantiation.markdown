---
author: andrewhao
comments: true
date: 2010-04-27 02:09:01
layout: post
slug: yui-widget-lazy-instantiation
title: YUI 3 Widget lazy instantiation
wordpress_id: 970
categories:
- Andrew 2.0
- Javascript
tags:
- Javascript
- js
- YUI
---

At work, we make good use of [YUI 3](http://developer.yahoo.com/yui/). It's a really well-thought-out framework, from sandboxing and deep namespaces to CSS3 selector support and lazy-loading modules through the Yahoo! CDN. One of YUI 3's biggest features is the Widget framework, which specifies an object on the page that the user can tweak to his or her whims.

Now typically, a YUI widget must be instantiated, then rendered onto the page. The render method is responsible for:



	
  1. Writing the widget's DOM elements out to the page

	
  2. Binding handlers onto the DOM object.

	
  3. Updating the widget state to match the JS object state.


(This information didn't come easily. I had to dig around in some YUI slides and found this in [Satyen Desai's "A Widget Walkthrough" presentation](http://developer.yahoo.com/yui/theater/video.php?v=desai-yuiconf2009-widgets) at YUIConf 2009.)

[![](http://www.g9labs.com/wp-content/uploads/2010/04/screenshot5-499x374.png)](http://www.g9labs.com/2010/04/27/yui-widget-lazy-instantiation/screenshot5/)

Now we have a page that generates hundreds if not thousands of these tooltips. Instantiating each tooltip, rendering them into the DOM and generating event handlers for each was wasteful and slowed IE browsers down to a crawl. So this brought us to the decision to lazily instantiate each Tooltip instance. My first thought was that I should be able to overwrite the render() method to write to the DOM only after a mouseover onto the Tooltip trigger.

Here's the folly: note that in the previous slide, the event handlers don't go live until after the widget HTML is rendered into the DOM. Rats. Is there any way I can get around this?

Here's some peculiar code for the Tooltip wrapper:

    
    function ttWrapper(triggerNode) {
      this.tt = null;
      /* First mouseover will instantiate the real TooltipWidget. */
      var mOver = Y.on('mouseover', function (e) {
        this.tt = new TooltipWidget(/*config*/).render();
        /* Detach this listener so it won't intercept any more mouseovers */
        mOver.detach();
        /* A second mouseover must be simulated on the real TooltipWidget */
        Y.Event.simulate(triggerNode, 'mouseover'); 
      }, triggerNode);



    
    }


The code installs a mouseover listener on the trigger node, then swaps itself out for the real mouseover listener that gets attached to the triggerNode when TooltipWidget gets instantiated. Event.simulate() is a strange, yet smart way to solve this problem.
