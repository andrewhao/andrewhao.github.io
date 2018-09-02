---
author: andrewhao
comments: true
date: 2011-08-10 09:25:10
layout: post
slug: introducing-boink-a-photobooth-for-the-rest-of-us
title: Introducing Boink, a photobooth for the rest of us.
wordpress_id: 1104
categories:
- Design
- Geek
- Interfaces
- programming
tags:
- project
---

My friends were complaining that wedding photobooths were too expensive to rent. Could we make one for them?

Glen and I from the [Porkbuns Initiative](http://porkbuns.net) stepped up in full armor, ready to help.

What is it? It's a self-running photobooth that uses your Mac for brains and DSLR for eyes and a Webkit browser for its clothes and a photo printer for... a printer. You can connect an iPad as the frontend for a nice visual touch (pun intended).

We built it on a backend Rails instance, pushing SVG+HTML5 in the frontend and using the gphoto4ruby gem as a camera library wrapper.

[caption id="attachment_1105" align="alignnone" width="500" caption="All dressed up and ready to go."][![](http://www.g9labs.com/wp-content/uploads/2011/08/284852_10100607611957573_1201208_60011361_805844_n-500x333.jpg)](http://www.g9labs.com/2011/08/10/introducing-boink-a-photobooth-for-the-rest-of-us/284852_10100607611957573_1201208_60011361_805844_n/)[/caption]

[caption id="" align="alignnone" width="500" caption="An early UI prototype."][![Boink Preview](http://farm7.static.flickr.com/6029/6006564205_60fda1b366.jpg)](http://www.flickr.com/photos/andrewhao/6006564205/)[/caption]

[caption id="attachment_1106" align="alignnone" width="500" caption="This comes out of the printer."][![](http://www.g9labs.com/wp-content/uploads/2011/08/gen_219-500x348.jpg)](http://www.g9labs.com/2011/08/10/introducing-boink-a-photobooth-for-the-rest-of-us/gen_219/)[/caption]


### Try it out


Check it out on [Github](http://www.github.com/andrewhao/boink).
