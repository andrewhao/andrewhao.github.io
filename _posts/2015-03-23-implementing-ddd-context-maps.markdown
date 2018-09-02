---
layout: post
title: "Implementing DDD: Context Maps"
date: 2015-03-23 10:56
comments: true
published: false
categories:
- DDD
- Domain Driven Design
- Implementing DDD
- Book
---

This is a multi-part series where I blog through the book: *Implementing
DDD* by Vaughn Vernon. Feel free to read my summary of [Chapter 2](/2014/04/26/implementing-ddd-chapter-2/) of
this series.

## Chapter 3: Context Maps

The context map is a diagram that illustrates the solution space of the
problem - it describes a diagram of "what already exists" as real,
living, breathing software.

Its purpose is to describe your specific solution space for your
specific team. It also has a benefit of facilitating conversations
between your team, and show you where inter-team communication is very
important.

### New terms:

* *Big Ball of Mud*: A messy, likely monolithic application that contains code that does not respect best practices, organization, or good coding practices.
* *Customer-Supplier*: The Customer is dependent on the Supplier's system.
* *Conformist*: A type of relationship where the Customer is forced to obey existing system conventions specified by the Supplier.
* *Partnership*: A coorperative relationship between two teams who succeed or fail together.
* *Shared Kernel*: A design strategy that shares some core code components. Care should be taken to define explicit boundaries around the kernel and keep the kernel small.
* *Anticorruption Layer*: A translation layer between two bounded contexts. Its job is to translate domain contexts between one domain and another.
* *Open Host Service*: A protocol that gives access to your system as a set of internal (or public) services, so others may reuse your code.

### Drawing a context map

Use a hand-drawn diagram, nothing fancy. Be sure to identify upstream
and downstream relationships. Talk to people. Keep it simple. Draw it
together, and have conversations.

It should turn up integration bottlenecks.

They should be posted in rooms or on a wall.
