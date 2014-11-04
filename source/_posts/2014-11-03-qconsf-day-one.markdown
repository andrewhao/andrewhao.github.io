---
layout: post
title: "QConSF: Day One"
date: 2014-11-03 09:21
comments: true
categories:
- qconsf
- conference
- functional programming
---

## Bruce Schneier: Security

* We don't have control over our infrastructure. Handing more data to the cloud
* THe future: we might see more vendor control over our OSes -- app store updates, etc. Windows 8, Yosemite look more like mobile OSes.
* Nowadays, there are a wider variety of attack(er)s. Hackers now have standard tools.
* Advanced persistent threats: Hackers just trying to grab a block of credit cards. Targeted threats: politically motivated hacking.
* IP theft as well as hacktivism
* "The entire supply chain is now complete." Stealing credentials -> turning account control

### Increased gov't involvment in cyberstates

* infrastructure
* legal involvement
* cyber arms race: people building offensive weapons

### Economics of IT

#### Network effects

Value of a network is the number of connections that can be made.

#### Fixed vs marginal cost

fixed cost is the development of the tool.
marginal cost is low. This is why it's profitable to steal 

#### Switching cost

Cost to switch from a competitive product

(IR: Instant response)

#### Describing psychology principle: [prospect theory](http://en.wikipedia.org/wiki/Prospect_theory)

We are risk averse when it comes to gains and we are risk seeking when it comes to losses. Evolutionary theory describes: short, guaranteed small gain is better for survival.

This makes security hard to sell: we got by OK last time. Why do we need security now? This is the boss flipping the coin. 

If we get hacked, we all have to leave and get a new job. THe economics for prevention are hard to sell. See analogy to selling insurance.

Principle: remove humans from the system as much as possible. Problem: humans cannot be completely eliminated from the process.

### Protection, detection and response

* Response is the arena where better products can do better. This is because technology is in charge to enable humans to detect and respond.
* Just like in public service: police, firemen, medical.

### Tools and OODA

* OODA loops: observe, orient, decide, act. Air Force captain. The faster you can eval this loop, the more advantage you have.
* What I want in IR is tools to get into the attacker's OODA loops.

#### Observe

* we need

tl;dr:
We will always underspend on preventative security
instant response tools is an arena of software tooling that the better products will beat out the worse

## Architecture at Netflix

* 20% of org is dedicated to maintaining platform -- internal PaaS
* Microservice architectures organized around teams
* Automate everything you can: tools, builds, images.
* You want your revenue to grow faster than your org
* Continuous integration - code deployed as soon as possible.
* Self service: Try to make everything as self-service as possible. Make it easy for engineers to set up their own deployments, their own monitoring.
* Monitoring: Do it from the beginning! Emitting events. See Etsy. Open sourcing the system soon. Make sure that they own the dashboard, so they can 
* Break things on production: Embrace failure. See: Netflix Simian Army.
* Incident review: Blameless culture

#### Data replication

* Shared state should not be stored within a shared service. You need to make sure there are failovers of your data. Multi data centers across AWS availability zones.
* Don't keep your secret keys on an instance.

#### Queues

Use queues for their operational value in helping you scale.

They give you a buffer between your users and your systems. You don't have to worry about the rate of change. They can buffer you from 

Monitor your queue lengths so you can scale.

#### Caching

Huge way to give users best experience. Write data to cache + queue.

#### Sharding

Splitting writes across multi servers based on data coming in

* Cassandra: used for consistent key hashing
multi data center
Think of SSD as cheap ram, not expensive disk

#### Lambda/Kappa Architecture

* Lambda: Idea of having two parallel data streams. One is accurate, slow, one is approximate, fast. Have queries combine the two.
* Kappa: Always use the correct stream

## LinkedIn Software Dev/Engineering

### Startup mode 

Systems built during startup lifetimes can't stand public growth.

300+ code projects in a single SVN repo

Testing code locally meant deploy every service locally!

24 hrs of integration testing - you find you broke master

3 hours writing code, you check it in, 3 days to commit it.

Do what the OSS does

monolith => individual git repos

### SDLC changes

* All code have owners
* pre-push checks

Canary builds - swaps one machine with new build with production traffic:

CPU/memory increases
fan-in fanout increases
error rate increases

### Architectural practices

Kafka is realtime signal ranking
HOw to solve for locality and multi data centers

IP routing based on location
How to solve data integrity? cassandra does it. linkedin used oracle + kafka.

Keep a local Kafka in each DC. Kafka just broadcasts to a global kafka

### LinkedIn Search

* LinkedIn archives several indices
* each search engine has its own search score
* LI tries to guess your intent when you search and query the right indices.
* query rewriting: "sr swe" can be "senior software eng" or "sr soft engineer"
* Galene Architecture: built on lucene. real time indexer, updates indices between hadoop builds

## Tumblr: bits to gifs

* sharding: jetpants
* php, haproxy, hdfs
* s3 for image storage

* Scala services: 90% of the time. Built in a world where Go didn't exist
* finagle -> colossus
* Thrift
* Protobuf
* hbase stores notifications
* firehose
* job queue: gearman

* Varnish caching: using DJB2 hashing for consistent hashes - balanced against

### Server management

* source of truth: Central repository of active servers
* Collins: self-serve servers. figure out what to provision
* "provision me a new server"

### Deployment

* deployment used to take an hour for 500 servers
* DUI is deployment tool
* Need 2 people from your team + staff/sr engineer
* get into an IRC deploy queue
* Deploy canary first
* Tumblr deploys over 40x a day
* Fibr is a graphing tool. so everyone can feel empowered to see root problems
* your engineers need to be empowered to understand tools.
* func is installed on any server at any point in time. "give me the server"

## FP at Twitter

* Key component is managing/reducing incidental complexity
* (scala): Futures: containers for value. Use composition operations.
* Presenter demonstrating how to build a search engine in scala with composable operations
* FP helps in "separating mechanics from semantics" - the bind function
* A service is an async function: takes a request and returns a Future reply.
* "your server as a function": http://monkey.org/~marius/funsrv.pdf

### Service discovery

* backed by zookeeper
* maintains a convergent view of the system
* zookeeper is hard to deal with. has strong guarantees.
* exploiting the declarative nature of these abstractions

### By modeling apps as composable services, you can reason well about your programs.

Example is finagle, which is just a composition of services

* "composability is one of the great ideas of functional"
* Good tool for enforcing modularity
