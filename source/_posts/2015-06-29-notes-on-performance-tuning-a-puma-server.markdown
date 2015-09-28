---
layout: post
title: "Notes on performance tuning a Puma server"
date: 2015-06-29 11:47
comments: true
categories:
- puma
- rails
- performance tuning
---

A couple of months ago, I was tuning a Rails app for one of our clients.
This client wanted to know how performant their app would be under load.

To do that, you can do several different things:

1. Tune the thread/process balance within the VM
2. Horizontally scale with your cloud platform.

This is a discussion of the former (#1):

## 1) Set up the test

### Drive with a synthetic script

Our application had a synthetic load driver that would run Selenium to
execute various app tasks. This synthetic driver could be parallelized
across many notes via Rainforest QA, Sauce Labs or Browserify.

In our case, I only needed to run our synthetic load script on a single
node in multiple processes, which simulated enough load to anticipate
another order of magnitude of traffic.

### Know how to inspect the server under load.

Commands you will want to know:

    $ free -m # Find the total amount of free memory on your machine
    $ ps uH p <pid> # List out process threads
    $ kill -TTIN <puma_master_pid> # Add a puma worker
    $ kill -TTOU <puma_master_pid> # Remove a puma worker
    $ kill -USR2 <puma_master_pid> # Kill the puma master & workers

## Generating more load: use external load testing services, or plain tools.

Try using [Flood.io](http://www.flood.io) or JMeter for performance load.

I tried looking into the [puma_auto_tune](https://github.com/schneems/puma_auto_tune) gem, but it required a higher level of production instrumentation than I was ready to give it.

## Analysis: New Relic scalability analysis

New Relic gave us a scalability analysis scatter plot, plotting
throughput against average application response time. In essence, it
allows you to see spikes in response times as correlated to throughput.

## Process:

My approach was to use the synthetic script to generate productionlike
node and ramp up the # of load actors in 5m increments. Each run would
test the following Puma process/thread balance:

Run #1: Single-process, multi threads.
Run #2: Multiple processes, single threaded.
Run #3: Multiple processes, multiple threads.

> ### Aside: *how many* of these threads/processes should I be using?
> 
> Note that your numbers will be different on the execution
> characteristics of your app and your server environment. Tweak it for
> yourself. You're designing an experiment.
>
> If you're curious, our Rails app started out with 4 threads on 2
> workers. We made the # of Puma workers (both min and max) environment
> variables so we could tweak the variables easily without deploying.

The strategy was then to look at the perf characteristics of each run in
the scatter plot. If there were any spikes in the graph with the
increase of load, then that would be noted. Even minor features like an
increase in slope would be noted - at that point, the incremental cost
of each request increases with overall system load.

## Results

I don't have the New Relic data on hand to show, now, but in our case we
discovered two things:

1. The server easily scaled from ~10 -> ~500 rpm with a virtually flat
   line for all runs.
1. The app exhibited no noticeable performance differences when flipped
   between uniprocess-multithreaded, multiprocess-unithreaded, and
   multiprocess-multithreaded modes. Any performance gains were under a
   tolerable threshold.

How do we parse these results?

* We note that we didn't really push the performance threshold on this
app (it's not meant to be a public web site and 95% of it is behind a
login wall to a specialized group of users). Thus, if we pushed the
concurrent connections even more, we may have seen more of a pronounced
difference.
* The *absence* of any major red flags was itself a validation. The
question we wanted answered coming into this experiment was "how close
are we to maxing out our single-node EC2 configuration such that we will
have to begin configuring horizontal scaling?"? The answer was: we can
safely scale further out in the near-term future, and cross the bridge
of horizontal scaling/bursting when we get there.
* We did not have enough statistically significant differences in
performance for #threads/#processes in Puma. However, if we wanted to
truly find the optimal performance in our app, we would have turned to
tools like [puma_auto_tune](https://github.com/schneems/puma_auto_tune) to answer those questions.

Let me know in the comments if you have any questions!
