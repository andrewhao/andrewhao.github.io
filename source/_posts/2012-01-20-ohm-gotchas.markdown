---
author: andrewhao
comments: true
date: 2012-01-20 18:19:50
layout: post
slug: ohm-gotchas
title: Ohm gotchas
wordpress_id: 1119
categories:
- Andrew 2.0
- programming
- Software
tags:
- activerecord
- blurb
- orm
- redis
---

Here's a list of things that have been annoying, or at least a bit frustrating using [Ohm](http://ohm.keyvalue.org), the Redis ORM, in a Rails app. Beware to those who assume Ohm is ActiveRecord in new clothes. It is, but it's not:



## CRUD



Don't make the mistake of treating your Ohm objects like AR:








  ActiveRecord
  Ohm






  
`destroy`

  
`delete`





  
`self.find(id)`

  
`self[id]`





  
`update_attributes`

  
`update`





  
`create`

  
`create`





Also note that Ohm's `update_attributes` behaves differently from Rails` -- it doesn't persist the updates to DB. That owned me for the good part of the day.



## Callbacks



Thankfully, these are ActiveRecord-like with the addition of [`ohm/contrib`](http://cyx.github.com/ohm-contrib/doc/).



## Associations










  ActiveRecord
  Ohm






  
`has_a` or `belongs_to`

  
`reference`





  
`has_many`

  
`collection`





Read [this article](http://blog.citrusbyte.com/2010/04/12/mixing-ohm-with-activerecord-datamapper-and-sequel/) if you're considering creating associations from AR objects to Ohm objects and the other way 'round.

