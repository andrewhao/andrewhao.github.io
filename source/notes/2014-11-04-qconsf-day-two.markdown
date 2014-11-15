---
layout: post
title: "QConSF: Day Two"
date: 2014-11-04 09:25
comments: true
categories: 
- qconsf
- conference
- notes
---
## Leslie Lamport: Programming is more than coding

* We should be thinking harder before we start coding. Clear thinking can prevent errors. Fuzzy/wishful thinking can't.
* How do you think clearly? Write.
* Specifications: help us think clearly.
* Think like a scientist!

### In computer science:

* reality is digital systems, processor chips.
* models are turing machines, tc..
* what is a program? code that makes sense to think about itself
* functions in math are not equal to functions in programming languages
* program execution represented as a behavior
* behavior is a sequence of states
* program is a sequence of behaviors

### Productivity

Best place to eliminate code is to think about what you need to do and what you *don't* need to do.

### How to describe a set of behaviors

* Theorems: behaviors can be described by safety and liveness properties.
* Safety property: something bad doesn't happen (doesn't throw exception)
* Liveness property: something good eventually does happen (program eventually returns)
* Errors more likely to occur in safety properties.
* current state, next state transition can be described by a formula

* Engineers starting to use TLA+ to describe system behaviors
* debug 6 lines of specs better than debugging 850+ LOC.
* when you write specs, you should write formal specs.
* Before you write code, write spec.
* Thus, you write better programs.

## Netflix: RxJava services

* Observables vs Iterables. Push vs Pull.
* Observable is at the core: abstraction of events over sets.
    * Prefer Observable over Future because Future is still fragmented in Java ecosystem.
* Using Rx services that aggregate granular APIs.

### Cold finite streams

* Shows an example of observable API - multiple network calls
* Instead of blocking APIs, can do async Observable APIs
* Decouples production from consumption. No matter how you implement
* Very cool example: a Collapser can batch up granular network calls in windows and send them off to a REST API.
* This lets you treat network calls like in-process methods, which goes against typical warnings in distributed systems theory
* Retrieval, transformation

### Error handling

* *Flow control*: You need backpressure when you hop threads. what do you do when you consume slower than producer? Often necessary in UIs
* *Hot vs Cold source*: Hot: emits whether you're ready or not. mouse event.Cold: emits when requested: HTTP request.
* Approach: block threads. like iterables.
* Hot streams: use temporal operators: like `sample` or `debounce`
* Buffer, debounce, buffer pattern - can group signals by temporal 
* *Reactive push: hot infinite stream*: Buffer by time window, drop some samples if appropriately, do map/reduce on windows

### Mental shift

* imperative -> functional
* sync -> async
* pull -> push
* Rx doesn't trivialize concurrency. You need to reason about what's going on underneath.

## Evolution to Reactive

### Monolith

* OK for small services, startups
* simple at first
* queries are easy
* "if you don't end up regretting your early tech decisions, you probably overengineered" (so it's OK!)


### Evolve to Microservices

Microservices: loosely-coupled service oriented architecture with bounded contexts

* single purpose
* simple well-defined interface
* modular and independent
* more a grpah of relationships than tiers
* isolated persistence!
* each unit is simple
* helps your company scale. people can hold it in their heads.

#### pros:
* unit is simple
* independent scaling and performance
* independent testing and deployment
* tune performance

#### cons:
* many cooperating units
* many small repos
* requires sophistication around tooling and dep management
* network latency

(Not a panacea for everything.)

### Google Services

* all groups are organized into services: gmail, app engine, bigtable
* self-sufficient and autonomous
* layered on each other -- empowers engineers to focus on the domains 

### Reactive microservices

* first tenet is to be responsive, fail latencies, async nonblocking calls from client
* resilient: redundancy, timeouts, retries. hystrix
* "release it" book my michael nygard
* elastic: can scale up and down according to load.
+ message-driven: message passing
* FRP patterns: actor model
* scala/akka + rxjava

### Kixeye service chassis

* Takes a long time to build and deploy a new service
* goal: make it easy to build/deploy new uservice
    * configuration
    * registration/discovery
    * load balancing for downstream services
    * failiure management for downstream svc
* eureka: service registry
* 15m no code to running service in AWS

### How to do this?

* Don't migrate in one big bang. do it incrementally.
* find your worst scaling bottleneck
* wall it off behind an interface
* replace it
* then do it again

### process
* common chassis
* define service interface. discuss with clients. agree
* prototype: simplest thing that could possibly work. client can integrate with prototype.
* distributed tracing -- track a request through multiple services
* network visualization
* dash metrics - scan health
* hystrix and turbine generate nice visualizations

### Other implications

* Can have smaller teams
* teams are autonomous, can make their own technology and implementation choices
* let the team own the HOW
* vendor/customer relationship: friendly, cooperative, but structured
* clear ownership and division of responsibility
* the customer can choose whether to use that service or not. if it's not the best choice for the client, why are we building/maintaining it?
* We should have market forces inside our companies than outside.

## Paypal API

Paypal used to have a fragmented, legacy API ecosystem.
Circa 2012, they began an initiative to standardize and modernize their API infrastructure.

### Defined a target state:
* API first
* API as a product
* REST first

### Architecture

* Built using facades
* Facades coordinate between internal services (microservices?)
* Tiered approach: experience APIs (facades) and capability APIs (core services)
* How did they assess success? Did a maturity model. Level 1-5
* People got into competitions
* Used DDD - bounded contexts - shared language
* Evolution is more than technology

