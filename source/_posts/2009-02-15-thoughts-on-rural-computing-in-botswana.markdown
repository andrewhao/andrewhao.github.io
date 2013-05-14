---
author: andrewhao
comments: true
date: 2009-02-15 14:02:41
layout: post
slug: thoughts-on-rural-computing-in-botswana
title: Thoughts on rural computing in Botswana
wordpress_id: 922
categories:
- Andrew 2.0
- Geek
- ict4d
- Web
tags:
- botswana
- Design
- ict4d
- IT
- rural computing
- web development
---

I'm currently in Botswana, doing volunteer work at a nonprofit Christian agency called [Love Botswana Outreach Mission](http://lovebotswana.org). Among my responsibilities are helping out with the organization's IT needs. I was pleasantly surprised to discover that the organization is wired for 24/7 Internet access, despite being located about 10km out from the center of Maun, a medium-sized village.


### Infrastructure


As far as I understand it, there is little-to-no telecom infrastructure here in Maun (i.e. there are no telephone, cable or fiber lines). Thus, most communication links happen via satellite or wifi radio.

The mission has a 100-ft radio tower, where a local ISP colocates a wireless repeater to hook into its wireless network. In exchange, the mission receives a special deal on its Internet access plan.


### Existing network setup


Because of the relatively low price and quick setup of wireless access points, the mission currently uses a mix of wired switches and wireless repeaters to extend the reach of the network.

A directional antenna on the tower beams a signal to a directional antenna mounted above the IT office trailer to an access point connected through a gateway serving as the DHCP server and firewall. From there, the signal gets sent via a a wired Ethernet network to the other office buildings.

Wireless access points have been installed at various points along the wired office network, allowing wireless access onto the Net.

Only recently has Internet connectivity been extended to reach a cluster of family homes, situated about 200 meters away from the office buildings. A wireless access point is set up in the IT trailer, broadcasting on a +10dB omnidirectional antenna. The closest house to the office has an outdoor +10dB omnidirectional antenna hooked up to a wireless bridge/repeater. Thus, the link from the houses to the gateway is maintained via a wireless bridge. The houses themselves are connected on a hardwired Ethernet network.


### Lessons from the field




#### Lesson: plan for environmental factors.


We observed that the wireless link from the homes to the offices would be intermittently reliable at best, often going down for no reason. We discovered later on that our link would fail on particularly humid days, because the signal quality over the wireless bridge would degrade significantly and decrease the range of the network.


#### Lesson: keep power voltages in mind.


I fried a power supply when I left it switched to a 115V (US) input, and plugged it into the 220V power plugs there. Sizzle. This led to a lot of apologizing and reassurances from the staff that this was a common thing for Americans to do.


#### Lesson: Clean computers frequently.


Dust is a common fact of life here. Thus, computer innards were frequently coated with dust, and more susceptible to overheating and/or a shorter lifecycle on moving parts (fans, etc).


#### Lesson: Scan for viruses, too


I don't know why, but viruses just seem to be more common here. I've been passed a few flash drives that have been infected. In the states, I'd be (un)lucky to find one a year.


#### Lesson: Plan for low bandwidth and outages


In the States, we depend on always-on Internet. Here, however, the Internet seems to go down for any of a multitude of reasons. If it _is_ up, it's being shared by fifteen computers all downloading at once over an already-restricted pipeline. Thus, Web pages can't be expected to load reliably, Skype sessions will cut out, and chat services won't be accessible.

I've come to depend on [Offline Gmail](http://gmailblog.blogspot.com/2009/01/new-in-labs-offline-gmail.html) to compose email when the Internet's down. Other productivity apps like [Remember the Milk](http://rememberthemilk.com) and [Google Docs](http://docs.google.com) offer offline modes for their applications.

I rely on Bonjour-enabled chat (iChat on a Mac, [Pidgin](http://pidgin.im) + [Bonjour for Windows](http://apple.com/support/downloads/bonjourforwindows.htm)) for the times when the Internet or DHCP server goes down and I need to chat with somebody in another office or house.

When making large-ish downloads, I've gone to using a download manager (flashback to the days of the 56K modem trying to download a 20 MB file)! I've been using [Free Download Manager](http://freedownloadmanager.org). It's not much of a name, but it's a full-featured open source program allowing you to schedule, pause, resume downloads.

Just FYI: my downloads range between 900 bytes/sec up to 28KB/sec.


#### Lesson: Designing Web pages for low bandwidth is still a good idea


As a Web developer, I tend to make the erroneous assumption people have the same quality of high-speed connection as I do back home in the States. Now that I'm a continent away from most of the sites I visit, I'm beginning to realize that optimizing a site for bandwidth savings can be a lifesaver. Here, a round trip to and from the States can take up to 20 seconds. That adds up over the course of a page load. As a Web designer or developer, ask yourself if you can be doing anything for the "56K guy": Are you using CSS sprites? A CDN? Compressing (gzip, mod_deflate) your output? Are you optimizing your image files for the Web?


#### Lesson: Knock out as many failure points as possible


Because the wireless bridge seemed to be the weakest link in our network, we decided to bypass it. We ran a really long Ethernet line from the IT office to the houses. This meant ordering a 200-meter length of cable, some sleeving and doing a lot of trench-digging to embed the cable under the dirt access roads. Our work paid off as everybody in the houses now enjoys a reliable network link to the main server (including me!).
