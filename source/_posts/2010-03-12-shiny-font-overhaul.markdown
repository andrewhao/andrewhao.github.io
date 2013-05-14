---
author: andrewhao
comments: true
date: 2010-03-12 11:54:15
layout: post
slug: shiny-font-overhaul
title: Shiny font overhaul!
wordpress_id: 953
categories:
- Andrew 2.0
---

### [![](http://www.g9labs.com/wp-content/uploads/2010/03/screenshot1-500x126.png)](http://www.g9labs.com/2010/03/12/shiny-font-overhaul/screenshot1/)


A couple of months ago I switched over to TypeKit, a cloud-hosted font service (buzzword! "cloud" gets me all warm and fuzzy). They basically take your font stack and enhance it with @font-face goodness, pulling in lots of fonts from their library.


### Why not roll a @font-face implementation yourself?


The availability of good, high-quality, _licensed_ fonts out there is still pretty small. The hoopla over @font-face has been centered around the fact that the fonts are unobfuscated, and anybody with the gumption to do so could go out to your site and steal yer font. So I'd say if you have a font you want to put up on the Web, you'd better make sure the font publisher/licenseholder is kosher with that.

(Ironically, Microsoft got it pretty right with their EOT font format, drafted way back in the days of IE 4 (!). Oh yeah, TypeKit takes care of EOT fonts for you as well.)

(Also, Mozilla is pushing their [WOFF format](http://hacks.mozilla.org/2009/10/woff/) which is currently supported in FF 3.6. Typekit does that too!).


### So, how's TypeKit working out for you?


Well, TypeKit's not bad. If you're running a cutting-edge browser (Safari 4, FF 3+, Chromium 4), you're going to see the font shininess. If you're not, you still get the default stack.

A problem way early on was the fact that Firefox for Windows was rendering certain fonts with very, very jagged features ([see comparisons](http://www.elated.com/articles/typekit-first-impressions/)). Turns out this was a fault of both the font and the OS/browser-- certain browsers and OSes don't render hints properly.

The best solution I've found? [Use a better font with better hinting](http://blog.typekit.com/2009/12/08/new-fonts-and-improved-hinting-from-our-partners/).

I'm using FF Nuvo Web and FF Enzo Web Pro:

[![](http://www.g9labs.com/wp-content/uploads/2010/03/Screenshot-g9Labs-Design-Studio-Typekit-Namoroka-500x412.png)](http://www.g9labs.com/2010/03/12/shiny-font-overhaul/screenshot-g9labs-design-studio-typekit-namoroka/)

All in all, I'm pretty satisfied.
