---
author: andrewhao
comments: true
date: 2008-03-12 18:46:29
layout: post
slug: after-site-duplication-an-htaccess-redirect-for-old-wordpress-posts
title: After site duplication, an .htaccess redirect for old Wordpress posts
wordpress_id: 867
categories:
- Andrew 2.0
- Geek
---

I think I must preface this post with a bit of context:

A month ago I added a new blog ([blog.andrewhao.com](http://blog.andrewhao.com)). I wanted that blog to become my personal blog and let this blog ([blog.g9labs.com](http://blog.g9labs.com)) transition into a technical blog.

Here comes a technical post about transforming a personal blog into a technical blog.

So here were the problem(s) I faced:



	
  * Most of my posts here at blog.g9labs.com were of personal nature, many which dated all the way back to 2002, my sophomore year of high school!

	
  * However, I had begun to write technical posts for a year or two--some thoughts on interface design, an occasional programming trick, cool regex expressions, et cetera.

	
  * Many of my posts have been linked all around the Internet.


Essentially, I wanted to:

	
  * Move the personal contents of the g9labs blog to the andrewhao blog,

	
  * While keeping all in-links to old g9labs content alive.


My solution was two-fold:

	
  1. I made a copy of my g9Labs* blog to the andrewhao blog. This was harder than I anticipated--I initially expected to simply use the WP export plugin to dump data from g9Labs* and import into andrewhao. However, this lost much of my metadata--comments, categories, tags. So I just decided to completely copy my g9Labs* WP database and WP directory into andrewhao. Yeah, hacky, I know. I'm not very proud of it.

	
  2. I added an .htaccess file to blog.g9labs.com that did some magic. It identifies requests for posts that are earlier than the date of the blog fork and redirects those requests to andrewhao.com, which is hosting a mirror of the post.



    
    
    # redirecting requests to posts older than 2008-02-02 to blog.andrewhao.com
    # pass the requester a 301 code (permanently removed)
    # @author: Andrew Hao
    
    # Lexically compare regex groups $1-$3 with the date: Feb. 03, 2008.
    # Note that the RewriteRule below is reached only if the condition is true.
    # Finally, note that $1-$3 are regex groups from the RewriteRule regex below.
    RewriteCond $1$2$3 <20080203
    
    # Match the incoming request: /YYYY/MM/DD/post-title and redirect the browser
    # to http://blog.andrewhao.com/YYYY/MM/DD/post-title
    RewriteRule ^([0-9]+)/([0-9]+)/([0-9]+)/([^/]+)/*$ http://blog.andrewhao.com/$1
    /$2/$3/$4/ [L,R=301]
    ... rest of .htaccess ...
