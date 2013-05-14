---
author: andrewhao
comments: true
date: 2012-12-26 12:46:09
layout: post
slug: decomposing-fat-models
title: Decomposing Fat Models
wordpress_id: 1171
categories:
- Rails
- Ruby
- Software Design
---

Heard an awesome Ruby Rogues podcast recently: ["Decomposing Fat Models"](http://rubyrogues.com/083-rr-decomposing-fat-models-with-bryan-helmkamp/).

Essentially, they're talking through Bryan Helmkamp's [Code Climate blog entry "7 ways to decompose fat ActiveRecord models"](http://blog.codeclimate.com/blog/2012/10/17/7-ways-to-decompose-fat-activerecord-models/), which sums up a few strategies that mainly involve extracting objects from your existing code, value, service, policy, decorator objects and the like. Give the entry a read-through, it's opened my eyes a lot to rethinking my architecture of my Rails models.

A few interesting thoughts that came up in the podcast:





  * The "Skinny Controller, Fat Model" mantra has hurt the Rails community because we start getting these bloated AR classes. "'fat-' anything is bad" one of the hosts mentions in the blog. The smaller your models, the more manageable, readable and testable they become.


  * Rubyists don't like the term "Factory", even though in Helmkamp's opinion, Ruby classes _are_ factories. "We call them "builders"" one of the hosts jokes.


  * The Open/Closed Principle as applied to Ruby: using delegators, decorators.



