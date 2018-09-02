---
author: andrewhao
comments: true
date: 2012-04-23 08:02:05
layout: post
slug: backup-backup-backup
title: Backup, backup, backup
wordpress_id: 1142
categories:
- Andrew 2.0
- Geek
- Linux
tags:
- adobe lightroom
- backup
- code
- dng
- git
- lightroom
- photography
- Photos
- rsync
---

Well, the inevitable happened: I finally experienced a hard drive failure. It's pretty incredible that in the twenty-odd years I've been around computers I've never had the horror of losing a drive.

Friday rolls around and my Macbook Pro decides to freeze up on me. _Strange, _I think to myself. It's making a clicking noise. Crap.

Luckily, I've been fairly good about making backups and copies of my work. Here's my general strategy:



	
  * Work/code: keeping local changes on a separate branch and pushing it to a remote Git branch every so often.

	
  * Everything else: I keep one local copy here with me in Oakland, and have another copy offsite. I rsync my files out to my server at home, which has a cronjob set up to sync with the offsite copy at my parents' home (I run a [Pogoplug](http://pogoplug.com/) with [Archlinux](http://archlinuxarm.org/platforms/armv6/pogoplug-provideov3) and a couple of external drives connected to it -- fantastic and totally recommended for a cheap and low-power server setup).


There was a minor scare this time around though -- I had some photography work (and an engagement photoshoot!) lying around that almost didn't make it to the first stage rsync with my local server. Fortunately, I had the foresight to keep my photos backed up to a random local hard disk, and the rest remained on the memory cards (and some even on a shared Dropbox folder that saved my butt!). Most frustrating thing was learning that I had forgotten to back up my Lightroom catalog, so all my edits were lost. At least I have the original shots.

One thing I think I'll try doing from here on out -- saving my Develop settings/presets directly to the DNGs themselves before backing up. That way if I ever lose my LR catalog, the edit settings are still embedded in the original files.

Jeff Atwood reminds us to [keep backups around on multiple disks](http://www.codinghorror.com/blog/2008/01/whats-your-backup-strategy.html). With the price of storage so low, what's your data worth to you? How are you keeping your backups?
