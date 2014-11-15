---
layout: post
title: "QConSF: Day Three"
date: 2014-11-05 09:22
comments: true
categories: 
- notes
- conference
- qconsf
---
## Melody Meckfessel: Google/Cloud Engineering

* The world is changing. Mobile is up and coming. Cloud platform is a huge market opportunity.
* Google DevOps platform: supports fast builds, caching.
* Best cloud platform on the planet (MarketingSpeak?)
* Every day, Google cranks out 800K builds, 2 petabytes of data...
* Internally: single monolithic code tree
* Code is open to any engineer
* Variety of languages
* Mandatory code review.
* Need good tools to get information about system.
* Deployment resource utilization

## Breaking the Monolith: Organizing your team to embrace microservices

* what about microservices for startups? 500px is 40-person startup that embraced microservices
* Ruby/Rails + Go architecture
* Conway's Law: reminds us that the core component of SE is humans!
* Microservices: "hipster SOA"?! small services. cost of integration is lower than we once thought.
* are SRP (single responsibility principle) at the codebase level

### Prior Architecture

* LB, app servers, db servers
* many data stores, one monolithic code
* lots of SPOFs
* Large codebase. hard to enforce boundaries, leak over time.
* lots of cascading failures
* deployments were "get in line"
* hosted datacenter

### First thing: changed search/infrastructure

* naturally asynchronous, off the critical path part of our infrastructure
* rewrote the indexer in go
* used to take 20hr to do a full build, now it takes 20m in Go
* wrote search service layer - extracted out of app

### Next: uploads

* EC2-based go uploaders
* Natural pair with S3

### Activities and notifications

* Ongoing
* Riak
* Technologies?

### Lessons

1) DevOps

* Do 12-factor apps
* Monitor everything. They use datadog -- hosted graphite
* Implement an on-call rotation
* Measure MTTD (detect), MTTR (resolve). Work to implement changes
* Test your failover scenarios

2) Automation

* Automation
* Use circuit breakers
* Michael Nygard: use circuit breakers. Try to recover from external failure scenarios.

3) Design for business capabilities

* Break app into biz capabilities (search, reco, uploads)
* Tempting to design around tech layers. don't
* treat each biz capability as a team

### Teams

* API facades can unblock teams. Web Team owns an API facade that proxies requests from the browser to serve the internal services (Sinatra app).
* Cute: BFFs - backend-for-frontend
* Web team: frontend code, API facade
* Media team: upload service, watermarking
* search team: search infrastructure
* Mobile team: mobile apps, mobile API facade
* Platform: everything else incl release engineering
* How to spread institutional knowledge? Swap developers.

## Deploying microservices with Docker, event sourcing, CQRS

@crichardson, http://microservices.io/

* Using scala, functional immudatable domain models, microservices, event sourcing, CQRS and Docker

### Why build event-driven microservices?

* Traditional architecture: ACID+WAR-bundled monolith
* Problem with monolith:
  * intimidates developers
  * infrequent deployments
  * overloads IDE
  * obstacle to scaling development
  * modules have different dependencies
  * long term commitment to architecture stack
* Apply functional decomposition
* Problem with RDBMS
    * Scalability
    * distribution
    * schema updates
    * O/R impedance mismatch
    * handling semi-structured data
* Microservices = distributed data management
    * very painful, don't try to do 2-phased commit
* NoSQL has no ACID guarantees, limited transactions, limited querying capabilities

### Event-based architecture to the rescue

* Microservices publish events when state changes
* Microservices subscribe to events
* for this to work, you have to atomically publish an event when state changes
* How to reliably generate events?
    * db triggers?
    * app code?
* How to atomically update the datastore and publish events?
    * use two-phased commit

### Event sourcing

* For each aggregate/entity:
  * identify all the state-changing domain events
  * define event classes
* That way you can reconstruct the current state of an entity by replaying events to get state.

* Persisting events: use weak serialization, like JSON
* For long-running aggregates, you can save a snapshot, discard old events.

### Biz benefits of Event Sourcing

* reliable audit log
* enables temporal queries
* publishes events needed by logs/big data
* solves data consistency issues. eventual consistency

### Drawbacks of event sourcing

* weird and unfamiliar
* events = history of bad events
* dupe events are tricky
* app handles eventually consistent data
* event store only supports PK-lookup of data

### Microservices

* Corresponds to a DDD aggregate
* requests from outside world => commands
* Processed by svc, generates events
* Can also subscribe to events over the bus

### CQRS

* commands process aggregates
* queries build views from events
    * view is a store
    * view updater service syncs the view with events
    * view query service generates the view from the view store

### How to deal with eventually-consistent views?

* creation update response contains aggregate
* out of date view compares version #s
* Or just fake it in UI

### Docker

* Jenkins deploy pipeline: Build & test code + build & test docker image + deploy docker to registry
* Smoke test the image:
    * `POST /containers/create`, `POST /containers/{id}/start`
    * ping `/health` URL
* tag image, then push image
* takes seconds to build, deploy!
* Jenkins is on Docker, too!

### Deployment to prod

* Diffs running dockers against build dockers, then deploys the changed containers.
* Mesos + marathon + zookeeper

## Culture of CareEvolution

* Completely decentralized. Decisions made by consensus
* culture of overwork in silicon valley
* Do we need Scrum masters? we don't trust people to do things themselves
* Self-directed teams can STILL apply to mission critical systems and important industries.

10. Individuals are center: process serves the individual. all individuals yearn to be *productive*.
9. Teams are nothing more than individuals working toward a common goal with trust in one another
8. if we can't trust each other then we need to start over. Lack of trust is a mortal weakness.
7. Management is not a title or role, but an activity to be performed by the individual. We are all managers! 
6. Work and life are not two opposing forces: it's both.
5. size brings complexity
4. ambiguity and the need for juddgment: embrace ambiguity. with time, the org is less capable of doing things, instead of doing more
3. you can't tie compensation to titles
2. Doing good is good business
1. Run the business as if it'll be there for 100+ years. Beyond you. So be wary of professional investors who want to see short term gain. Question if there really _is_ a first mover advantage.

### Netflix: "Responsible person"

* Self-motivating
* Self-aware
* Self-disciplined
* Self-improving
* Acts like a leader
* Doesn't wait to be told what to do
* Never feels "that's not my job"
* picks up the trash lying on the floor
* Behaves like an owner

## Melissa Pierce: 

* Grace Hopper: Ph.D in mathematics at Yale
* Babbage machine
* 1842-1843: she wrote elaborate set of notes
* Look at ads and how they portray women. Women as commodities, cheap
* women were strong in math. it wasn't perceived as being professional enough. engineering was masculine. CS moved to be an "engineering" degree
* the drop and decline in women in CS happened around the time of its alignment with engineering