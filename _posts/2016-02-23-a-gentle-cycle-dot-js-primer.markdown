---
layout: post
title: "A gentle Cycle.js primer"
date: 2016-02-23 22:01
comments: true
published: false
categories:

---

Cycle.js

HCI
Dialogue loop interaction

DOM <-> Human <-> DOM
Motion <-> App
Rickshaw <-- App

Your inputs probably need to be `share()`d. This is because multiple
subscribers.

RxJS converts cold to hot with `publish().refCount()`, aliased to
`share()`
