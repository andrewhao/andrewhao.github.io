---
layout: post
title: "Strange Loop 2015: Notes & Reflections"
date: 2015-09-27 15:40
comments: true
categories:
- conference
- strangeloop
- functional programming
- software
- distributed systems
---

Going to Strange Loop was a huge check off my conference bucket list
(lanyard?). I'd always heard about this slightly-weird, highly academic
collision between academia and industry, skewing toward programming
languages you haven't heard of (or, at the very least, you've never used
in production). I anticipated sitting at the feet of gray-haired wizards
and bright-eyed hipsters with Ph.Ds.

The conference did not disappoint. And it was not quite what I
expected-I less sat at the feet of geniuses than I did talk with them,
peer-to-peer, about topics of interest. All around me people were saying
"Don't be afraid to ask questions. Don't feel stupid - nobody knows
everything." Speakers were tweeting about how much they were learning.
It was comforting, because lots of topics I had come to see were those
in which I had no. freakin. clue. about.

The following is culled from my notes from different sessions I
attended. I will focus on brevity. I will keep it clear. Here we go:

### Opening Keynote: "I see what you mean"

* Instructions, behaviors & outcomes.
* It "feels good" to write in C (a hardcord 1000 liner)
* But a declarative program (e.g. SQL) works well, but is harder to come
up with.
* The declarative world - as described in the work done in Datalog
* How can we take concepts from Datalog and apply to real-world
resources like network actors (distributed systems)?
* It becomes easier to model these systems declaratively when we
explicitly capture time.
* Enter Dedalus: extension to Datalog where time is a modeling
construct.
* (Show off usage of `@next` and `@async` annotations
* Computation is redezvous - the only thing that you know is what YOU
know at that point in time.
* Takeaway: Abstractions leak. Model them better (e.g. with time)
* Inventing languages is dope.

### Have your Causality and your Wall Clocks Too (Jon Moore)

- Take concept of Lamport clocks and extend them with hybrid clocks.
- And extend them one further with: Distributed Monotonic Clocks
- These DMCs use population protocol (flocking) to each actor in the
system communicate with another, updating their source of truth to
eventually agree on a media time w/in the group
- DMC components:
  1. Have a reset button by adding epoch bit
  2. Use flocking (via population protocol) to avoid resets
  3. Accomodates for some clockless nodes
  4. Explicitly reflects causality

### Isomorphic JS
- Vevo needed better SEO for SPAs. Old soln was to snapshot page and upload to S3.
- Beneficial for SEO crawlers
- React in frontend. Node in backend.
- Vevo-developed [pellet]() project as Flux-like framework to organize
files.
- Webpack aliases/shims
- Server hands off to browser, bootstraps React in client.
- Alternatives: Relay, Ember

### Designing for the Worst Case: Peter Bailis (@pbailis)
- Designing for worst case often penalizes average case
- But what if designing for the worst case actually helps avg case?
- Examples from dstbd systems:
  - Worst case of disconnected data centers, packet loss/link loss. Fix
  by introducing coordination-free protocols. Boom, you've now made your
  network more scalable, performant, resistent to downtime.
  - Worst case: hard to coordinate a distributed transaction between
  services. What do you do? You implement something like buffered writes
  out of process.
    - CRDT, RAMP, HAT, bloom
    - Suddenly, you have fault tolerance
  - Tail latency problem in microservices: the more microservices you
  query, the higher the probability of hitting a slow server response.
    - Your service's corner case is your user's average case
  - HCI: accessibility guidelines in W3C lift standards for all. Make
  webpages easier to navigate. Side effect of better page performance,
  higher conversion.
  - Netflix designing CC subtitles also benefits other users.
  - Curb cuts in the real world to help ADA/mobility-assisted folks also
  benefit normal folks too
- Best has pitfalls too: your notion of best may be hard to hit, or
risky. You may want to optimize for "stable" solution. (Robust
optimization)
- When to design for worst case?
  - common corner cases
  - environmental conditions vary
  - "normal" isn't normal
- worst forces a conversation
  - how do we plan for failures?
  - what is our scale-out strategy?
  - how do we audit failures? data breaches?

### Ideology by Gary Bernardt
- Rumsfeld: known knowns, known unknowns, and unknown unknowns.
- Ideology is the thing you know you do not know you know
- Conflict between typed vs dynamic programmers:
  - Typed: "I don't need tests, I have types"
  - Dynamic: "I write tests, so I don't need types"
- In reality, they are solving different places in the problem domain,
but they have different beliefs about the world that are hidden in the
shadows:
  - Typed: "Correctness comes solely from types"
  - Dynamic: "Correctness comes solely from example"
