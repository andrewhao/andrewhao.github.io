---
author: andrewhao
comments: true
date: 2008-11-05 21:21:06
layout: post
slug: setting-up-a-multi-ap-wireless-network
title: Setting up a multi-AP wireless network
wordpress_id: 918
categories:
- Geek
tags:
- ap
- dlink
- ech
- Geek
- netgear
- router
- wifi
- wireless
---

A few weeks ago, I couldn't seem to connect my laptop to the wireless AP in my apartment. Additionally, I'd be getting weak signals from all these weird corners in my apartment. Since I had a spare AP lying around at home, I decided to do some online research to see if I could get it to behave as a second wireless AP on the same network, extending my network range, allowing our network to handle more wireless users and playing nicely with my laptop.


#### The desired setup:





	
  * Two wireless routers on the same network: one router acts as the gateway and DHCP server, the other is a simple switch + wireless AP. One router is the Netgear WGR614, the other is the DLink DI-624.

	
  * Workload: many wired connections, many wireless connections. We have 3 wired computers for me and my roommates' computers, and up to 10 people connected via wireless at the same time (we tend to have a lot of visitors).


After a bit of research, I [found a forum thread online](http://www.dslreports.com/forum/remark,10664212) that details how to set it up:



	
  * The host router (the Netgear in my case) stays the DHCP server and broadband gateway. Settings:

	
    * Assign a static IP on the MAC address of the secondary router. It's 192.168.1.3 on my network. That way you'll always have an IP address to the second router's admin panel.




	
  * Run an Ethernet cable from the host router's LAN port in to the LAN port of the second router (the DLink). (Note: it's especially important that you plug this wire in to the LAN port instead of the WAN port).

	
  * With a computer connected on the network, send your browser on over to the D-Link admin panel at 192.168.1.3. Your settings on the DI-624 should be as follows:

	
    * "WAN Settings" section (Note the 20.2.20.x IP is just a random IP address so the AP doesn't go off and try to dynamically resolve a IP address)

	
      * "Static IP Address" radio button selected

	
      * IP Address: 20.2.20.20

	
      * Subnet mask: 255.255.255.0

	
      * ISP Gateway Address: 20.2.20.21

	
      * Primary DNS address: 20.2.20.22




	
    * "LAN Settings" section:

	
      * Static IP: 192.168.1.3

	
      * Subnet mask: 255.255.255.0

	
      * DNS Relay: Disabled.




	
    * "DHCP Settings" section:

	
      * DHCP Server: Disabled




	
    * "Wireless" section:

	
      * Set up your wireless AP settings however you'd like!








Though these instructions are specific to setting up the DI-624, I'm pretty sure you could get your old router to work on an existing network.You can now extend the range of your home's wireless network without any fancy signal booster tricks or expensive repeater equipment. How cool is that?
