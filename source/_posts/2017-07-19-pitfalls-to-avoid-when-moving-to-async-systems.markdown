---
layout: post
title: "Pitfalls to avoid when moving to async systems"
date: 2017-07-19 19:57:18 -0700
comments: true
categories: 
- DDD
- Rails
- Microservices
---

I recently published a post on the [Carbon Five blog](http://blog.carbonfive.com) titled ["Evented Rails: Decoupling complex domains in Rails with Domain Events"](http://blog.carbonfive.com/2017/07/18/evented-rails-decoupling-complex-domains-in-rails-with-domain-events/) that takes some of my thoughts about moving a Rails app to use Domain Events - leveraging the power of Sidekiq (or your job runner of choice) to send async messages between different domains of your app.

This approach always seems nice from the outset, but can hide some painful complexities if you go too far down the rabbit hole. Here is a repost of the latter half of that article, which is worth repeating:

## Big win[s of the async model]: speed & scalability

By splitting out domain logic into cohesive units, we’ve just designed our systems to farm out their workloads to a greater scalable number of workers. Imagine if you had a web request thread that would take 500ms to return, but 150ms of that time was spent doing a round trip to a different service. By decoupling that work from the main request thread and moving it to a background job – we’ve just sped up the responsiveness of our system for our end user, and we know that studies have shown that page speed performance equals money.

Additionally, making our application calls asynchronous allows us to scale the number of processing power we allocate to our system. We now have the ability to horizontally scale workers according to the type of job, or the domain they are working from. This may result in cost and efficiency savings as we match processing power to the workload at hand.

## Big challenge: dealing with asynchronous data flows

Once things go async, we now have a fundamentally different data design. For example, say you implemented an HTTP API endpoint that performed some action in the system synchronously. However, now you’ve farmed out the effects of the action to background processes through domain events. While this is great for response times, you’ve now no longer got the guarantees to the caller that the desired side effect has been performed once the server responds back.

### Asynchronous polling

An option is to implement the Polling pattern. The API can return a request identifier back to the caller on first call, with which which the caller can now query the API for the result status. If the result is not ready, the API service will return with a Nack message, or negative Ack, implying that the result data has not arrived yet. As soon as the results in the HTTP API are ready, the API will correctly return the result.

### Pub/Sub all the way down

Another option is to embrace the asynchronous nature of the system wholly and transition the APIs to event-driven, message-based systems instead. In this paradigm, we would introduce an external message broker such as RabbitMQ to facilitate messages within our systems. Instead of providing an HTTP endpoint to perform an action, the API service could subscribe to a domain event from the calling system, perform its side effect, then fire off its own domain event, to which the calling system would subscribe to. The advantage of this approach is that this scheme makes more efficient use of the network (reducing chattiness), but we trade off the advantages of using HTTP (the ubiquity of the protocol, performance enhancements like layered caching).

Browser-based clients can also get in on the asynchronous fun with the use of WebSockets to subscribe to server events. Instead of having a browser call an HTTP API, the browser could simply fire a WebSocket event, to which the service would asynchronously process (potentially also proxying the message downstream to other APIs with messages) and then responding via a WebSocket message when the data is done processing.

## Big challenge: data consistency

When we choose an asynchronous evented approach, we now have to consider how to model asynchronous transactions. Imagine that one domain process charges a user’s credit card with a third party payment processor and another domain process is responsible for updating it in your database. There are now two processes updating two data stores. A central tenet in distributed systems design is to anticipate and expect failure. Let’s imagine any of the following scenarios happens:

1. An Amazon AWS partial outage takes down one of your services but not the other.
1. One of your services becomes backed up due to high traffic, and no longer can service new requests in a timely manner.
1. A new deployment has introduced a data bug in a downstream API that your teams are rushing to fix, but will requiring manual reconciling with the data in the upstream system.

How will you build your domain and data models to account for failures in each processing step? What would happen if you have one operation occur in one domain that depends on data that has not yet appeared in another part of the system? Can you design your data models (and database schema) to support independent updates without any dependencies? How will you handle the case when one domain action fails and the other completes?

### First approach: avoid it by choosing noncritical paths to decouple, first

If you are implementing an asynchronous, evented paradigm for the first time, I suggest you carefully begin decoupling boundaries with domain events only for events that lie outside the critical business domain path. Begin with some noncritical aspect of the system — for example, you may have a third party analytics tracking service that you must publish certain business events to. That would be a nice candidate to decouple from the main request process and move to an async path.

### Second approach: enforce transactional consistency within the same process/domain boundary

Although we won’t discuss specifics in this article, if you must enforce transactional consistency in some part of your system (say, the charging of a credit card with the crediting of money to a user’s account) then I suggest that you perform those operations within the same bounded context and same process, leaning on transactional consistency guarantees provided by your database layer.

### Third approach: embrace it with eventual consistency

Alternatively, you may be able to lean on “eventual consistency” semantics with your data. Maybe it’s less important that your data squares away with itself immediately — maybe it’s more important that the data, at some guaranteed point in time — eventually lines up. It may be OK for some aspect of your data (e.g. notifications in a news feed) and may not be appropriate for other data (e.g. a bank account balance).

You may need to fortify your system to ensure that data eventually becomes consistent. This may involve building out the following pieces of infrastructure.

1. Messages need to be durable — make sure your job enqueuing system does not drop messages, or at least has a failure mode to re-process them when (not if!) your system fails.
1. Your jobs should be designed to be idempotent, so they can be retried multiple times and result in the correct outcome.
1. You should easily be able to recover from bad data scenarios. Should a service go down, it should be able to replay messages, logs, or the consumer should have a queue of retry-able messages it can send.
1. Eventual consistency means that you may need an external process to verify consistency. You may be doing this sort of verification process in a data warehouse, or in a different software system that has a full view of all the data in your distributed system. Be sure that this sort of verification is able to reveal to you holes in the data, and provide actionable insights so you can fix them.
1. You will need to add monitoring and logging to measure the failure modes of the system. When errors spike, or messages fail to send (events fail to fire), you need to be alerted. Once alerted, your logging must be good enough to be able to trace the source and the data that each request is firing.

The scale of this subject is large and is under active research in the field of computer science. A good book to pick up that discusses this topic is [Service-Oriented Design with Ruby on Rails](https://www.amazon.com/Service-Oriented-Design-Rails-Addison-Wesley-Professional/dp/0321659368). The popular [Enterprise Integration Patterns](http://www.enterpriseintegrationpatterns.com/) book also has a great topic on consistency (and is accompanied by a very helpful [online guide](http://www.enterpriseintegrationpatterns.com/patterns/conversation/EnsuringConsistencyIntro.html) as well).
