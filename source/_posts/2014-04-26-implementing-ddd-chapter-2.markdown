---
layout: post
title: "Implementing DDD: Chapter 2"
date: 2014-04-26 08:43
comments: true
categories: 
  - DDD
  - Book Review
  - Domain-Driven Design
---

## Chapter 2: Domains, Subdomains, and Bounded Contexts

A *Domain* is what a business does and the surrounding context of how it does business. It is important to model out its supported *Subdomains* -- that is, the smaller components of the business that collaborate in real, day-to-day operations of the business. The book describes these as *Core Domains*.

Finally, a *Bounded Context* is the physical manifestation of the solution space as software -- models living in a real application. Its key feature in the context of DDD is as a linguistic barrier between different domains.

In an ideal world, each *Subdomain* maps directly to one *Bounded Context*. In the real world, this is less common since we tend to build things into monolithic systems. Still -- many monolithic applications have several components that could in themselves be bounded contexts.

###### Side note: In Rails, one could think of Engines as a *Bounded Context*. But that might be a blog post for another time.

It is important to get these ideas and concepts down correctly because we need correct modeling of our systems to determine what they do.

### Bounded Contexts and terms

It's not usually realistic to get the entire organization agreeing on a universal linguistic definition for every term. Instead, DDD assumes that different terms have different meanings in different contexts.

The author then dives into the an example of a book, where a *book* means several different things to different people in different contexts. A book is touched upon by authors, graphic designers, editors, marketing folks. In each of these contexts, the features of a book mean different things at different times. It is impossible to develop an all-knowing Book model without disagreement between stakeholders. DDD, instead, acknowledges these differences and allows stakeholders to use linguistic terms from within their unique Bounded Contexts.

Bounded Contexts may include things like:

* Object models
* A database schema / persistence layer
* A SOAP or REST API
* A user interface

### Bounded context antipatterns

You may be tempted to divide up a bounded context by architectural concerns, or because you want to assign smaller tasks to developers (resource allocation). Beware that this kind of work tends to fragment the language.

DDD operates on *linquistic drivers*. The unity, cohesion and domain-adherence of the bounded context should be the first priority in the design.

Assigning two teams to the same bounded context results in fragmentation of the bounded context.

Ideally: we strive to assign one team to work on one Bounded Context with one Ubiquitous Language at a time.

###### In Rails, what are the bounded contexts? It could be the top-level Rails application, or an engine, or a gem, that define the context boundaries._

### A story...

The chapter then goes on to describe their fictional team designing through three iterations off their DDD strategy:

A system in which all domains live within the same bounded context. They see the folly of this and refactor with some tactical patterns, like creating services.

This is, however, missing the point. They realized they needed to listen to business and their domain experts to find out exactly where the right places were to segregate the contexts. The team discovers that the business has a desire to go in a new direction which allows them to segregate the domain in such a way that would enable future directions for the business.

###### How often are we as developers in conversation with our product owners and asking where the business *wants* to go in the future?

Conversations with the business reveal an intention to develop an add-on product to the core product. This implies the development of two subdomains. However, further investigation reveals that the shared overlap of certain domain models (like users, roles, and access management) cannot simply be identically shared between two systems, since their *linguistic meanings* in the two systems differ slightly. Instead, the developers use the linguistic problem to develop a third bounded context.

The developers separate their app into three bounded contexts built as three software systems.