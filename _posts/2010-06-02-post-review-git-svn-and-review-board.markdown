---
author: andrewhao
comments: true
date: 2010-06-02 11:26:02
layout: post
slug: post-review-git-svn-and-review-board
title: post-review, git-svn and Review Board
wordpress_id: 990
categories:
- Andrew 2.0
- Geek
- Linux
- programming
---

Here's how to set up the excellent VMware-developed open-source [Review Board](http://www.reviewboard.org/) and its `[post-review](http://www.reviewboard.org/docs/releasenotes/dev/rbtools/0.2/)` command line review creation utility to work with git and git-svn on your computer.

My assumption is that you're working with a local Git repository that is remotely linked to an SVN repository via the [git-svn](http://utsl.gen.nz/talks/git-svn/intro.html) bridge. Let's assume that your master branch is synced with the SVN repository, and you're working on `bug_12345_branch`.



	
  1. Make sure you have RBTools installed (`sudo easy_install rbtools` for me on Ubuntu Linux), and Review Board set up elsewhere.

	
  2. Add a link to your Review Board URL in .git/config:




> 

>     
>     [reviewboard]
>      url = <a href="#">https://(url_for_review_board)/</a>
> 
> 






	
  1. Make sure all your changes in `bug_12345_branch` have been locally committed.

	
  2. `post-review -o`, and...

	
  3. Voila! You should have a new review up on your Review Board instance.

	
  4. (After you get reviews, you can modify `bug_12345_branch`, pull the changes into master, and then `git svn dcommit`, blah, blah, blah.)


