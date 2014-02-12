---
layout: post
title: "Blogging through: Implementing Domain-Driven Design"
date: 2014-02-02 11:31
comments: true
categories: 
- Book Review
- Blog A Book
- Domain-Driven Design
- DDD
---
In recent conversations with coworkers, the topic of Domain-Driven Design has
arisen on more than a few occasions in design and architecture meetings.
"Have you read it?" a coworker asked, "I think it'd help us a lot."

I've gotten my hands on a copy of [Implementing Domain-Driven Design](http://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)
by [Vaughn Vernon](https://vaughnvernon.co/), which is a more pragmatic
approach to DDD than the original [Domain-Driven Design](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/ref=pd_bxgy_b_text_y) book by Eric Evans.

My desire is to share my outlines of the book chapter-by-chapter,
hopefully once a week.

## Chapter 1: Getting Started with DDD

### Can I DDD?
* DDD helps us design software models where "our design is exactly how
  the software works" (1). 
* DDD isn't a technology, it's a set of principles that involve
  discussion, listening, and business value so you can *centralize
  knowledge*.
* The main principle here is that we must "understand the business in
  which our software works" (3). This means we learn from domain experts
  in our field.
* What is a domain model? an object model where objects have
  data/persistence concerns with an accurate business meaning.

### Why You Should Do DDD

* Domain experts and devs on same playing field, cooperation required as
  one team. (Agile teams, anyone?)
* The business can learn more about itself through the questions asked
  about itself.
* Knowledge is centralized.
* Zero translations between domain experts and software devs and
  software.
* "The design is the code, and code is the design." (7)
* It is not without up-front cost

#### The problem

* The schism between business domain experts and software developers
  puts your project (and your business) at a risk.
* The more time passes, the greater the divide grows.

#### Solution

* DDD brings domain experts and software developers together to develop
  software that reflects the business domain mental model.
* Oftentimes this requires that they jointly develop a "Ubiquitous
  Language" - a shared vocabulary and set of concepts that are jointly
  spoken by everybody.
* DDD produces software that is better designed & architected -> better testable ->
  clearer code.
* Take heed: DDD should only be used to simplify your domain. If the net
  cost of implementing DDD is only going to add complexity, then you
  should stay away.

#### Domain model health
* As time passes, our domain models can become
  [anemic](http://www.martinfowler.com/bliki/AnemicDomainModel.html),
  and lose their expressive capabilities and clean boundaries. This can
  lead to spaghetti code and a violation of object responsibilities.
* Why do anemic domain models hurt us? They claim to be well-formed
  models but they hide a badly designed system that is still unfocused
  in what it does. (Andrew: I've seen a lot of Service objects that
  claim to be services but really are long scripts to get things done.
  There might be a cost of designing the Service interface, but inside
  things are just as messy as before we got there.)
* Seems like Vernon is blaming the influence of IDEs for Visual Basic as
  they influenced Java libraries -- too many explicit getters and
  setters.
* Vernon throws up some code samples comparing two different code
  samples -- one with an anemic model that looks like a long string of
  commands and another with descriptive method names. He makes the case
  that simply reading the code is documentation of the domain itself.

#### How to do DDD

* Have a [*Ubiquitous Language*](http://martinfowler.com/bliki/UbiquitousLanguage.html)
  where the team of domain experts share the language together, from
  domain experts to programmers.
* Steps to coming up with a language:

   1. Draw out the domain and label it.
   2. Make a glossary of terms and definitions.
   3. Have the team review the language document.

* Note that a Ubiquitous Language is specific to the context it is
  implemented in. In other words, there is one Ubiquitous Language per
  Bounded Context.

### Business value of DDD

1. The organization gains a useful model of its domain
2. The precise definition of the business is developed
3. Domain experts contribute to software design.
4. A better user experience is gained.
5. Clean boundaries for models keep them pure.
6. Enterprise architecture is better designed.
7. Continuous modeling is used -- the working software we produce is the
   model we worked so hard to create.
8. New tools and patterns are used.

#### Challenges

* The time and effort required to think about the busines domain,
  research concepts, and converse with domain experts.
* It may be hard to get a domain expert involved due to their
  availability.
* There is a lot of thought required to clarify pure models and do
  domain modeling.

#### Tactical modeling

* The *Core Domain* is the part of your application that has key and
  important business value -- and may require high thought and attention
  to design.
* Sometimes DDD may not be the right fit for you -- if you have a lot of
  experienced developers who are very comfortable with domain modeling,
  you may be better off trusting their opinion.

#### DDD is not heavy.

* It fits into any Agile or XP framework. It leans into TDD, eg: you use
  TDD to develop a new domain model that describes how it interacts with
  other existing models. You go through the red-green-refactor cycle.
* DDD promotes lightweight development. As domain experts read the code, they
  are able to provide in-flight feedback to the development of the
  system.

