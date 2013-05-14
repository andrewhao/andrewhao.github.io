---
author: andrewhao
comments: true
date: 2008-02-18 17:32:42
layout: post
slug: todays-geek-hijinks
title: Today's Geek Hijinks
wordpress_id: 864
categories:
- Andrew 2.0
- Design
- Geek
- Linux
tags:
- Design
- dns
- dreamhost
- g9labs
- google
- Linux
- samba
---




### Morning: Woke up and attempted to mount a Samba file share under Ubuntu.


My Gutsy server in the living room is getting a second life as a backup file server. I need to mount it in my filesystem so I can rsync certain directories on my desktop over to the server.

`mount -t //my_server_ip/share_folder /media/mount_folder`

Alas! I couldn't get write access into the mount point! At first I thought this was a chmod problem, so I added myself as the mount_folder owner.

`sudo chown username mount_folder/`

To no avail. So then I attempted to use the "permit all" chmod sledgehammer:

`sudo chmod 777 mount_folder/`

Argh! STILL no good!

So then I went back to the drawing board and did some investigating. I could, 50% of the time, gain write access to child subfolders on the Samba share via GNOME Nautilus interface (smb://my_server_ip). Weird! Was this a Samba config problem? I didn't have the cajones to find out.

As a last-ditch attempt, I tried to modify my fstab file to mount the Samba share (I read somewhere that the uid and gid option flags give you the win). Side note: you can find your username's uid and gid numbers from the `/etc/passwd file`.

    
    
    # TEMPORARY SAMBA SHARE FROM LIVINGROOM SERVER
    
    # @author Andrew
    
    //my_server_ip/archive /media/archive smbfs noauto,rw,uid=1000,gid=1000 0 0


_Still_ no good! Arghs! At this point, I gave up and simply SFTP-ed in to dump my files. Admitting defeat is a sad, sad thing.


### Afternoon: Designed some logo comps for a Web company.


[![Logo I started on.](http://blog.g9labs.com/wp-content/uploads/2008/02/logo.png)](http://blog.g9labs.com/wp-content/uploads/2008/02/logo.png)

I'm working under contract from Ryan Waliany. This site should be doing some cool things. Unfortunately, I can't reveal too much. All this to say that I've taken a long hiatus from design and I really miss it.


### Late Afternoon: Set up Google Apps for g9labs.com


I gave in to the hype, and began setting up a Google Apps For Your Domain (GFYD) account for this g9labs site, primarily so I can hook into the awesome GMail interface instead of being chained to the Thunderbird client.

This was a multi-step process of:



	
  1. **Verifying ownership of your domain. **Google has you modify a CNAME entry on your domain so they can ping it to check that it's changed. I marched happily over to the [Dreamhost](http://dreamhost.com) panel where I found the DNS page for [g9labs.com](http://g9labs.com). There I added a CNAME value of googlexxxxxxxxx (a unique value given to you) and set its CNAME value to "google.com".

	
  2. While Google takes 48 hours to verify you've done the right steps, you can go ahead and **modify the MX records for your domain **so mail gets sent on over to Google instead of your Dreamhost server. This was easily set in the Dreamhost panel under Mail >> MX Records. You change your MX records to Google's special MX server, and you're on your way!

	
  3. I'm waiting for verification to go through, then I'll talk about how to get **IMAP for GFYD working**.


