---
author: andrewhao
comments: true
date: 2011-02-15 05:29:07
layout: post
slug: arduino-and-python-firmata
title: Arduino and python-firmata
wordpress_id: 1031
categories:
- Geek
- programming
- SOS
tags:
- arduino
- computer
- firmata
- hacking
- pde
- python
---

I just spent five hours trying to figure out why  none of the [Firmata libraries for Python](http://arduino.cc/playground/Interfacing/Python) were working over my serial connection. I was wondering why the previous program remained on the board and none of the signals sent were hot.

Hint: you need to load Firmata first onto the board before it can understand the protocol. Oh. Duh.

[caption id="attachment_1032" align="alignnone" width="500" caption="Don't forget to load up "OldStandardFirmata""][![](http://www.g9labs.com/wp-content/uploads/2011/02/Screen-shot-2011-02-15-at-4.24.47-AM-500x312.png)](http://www.g9labs.com/2011/02/15/arduino-and-python-firmata/screen-shot-2011-02-15-at-4-24-47-am/)[/caption]

Only "OldStandardFirmata" (Firmata 2.0) seems to work with my version of [python-firmata](https://github.com/lupeke/python-firmata). The newer Firmata PDEs can talk Firmata 2.1 and 2.2, but I'm too tired to figure them out.
