---
layout: post
title: A Staff Engineer's Survival Guide to Big Tech
date: 2025-01-01 00:53 -0800
categories:
- Career
- Staff Engineering
- Big Tech
---


<h2 class="intro">After a career built around startups, scale-ups and consulting, I left Lyft and landed a role in Google (YouTube) in 2022. Two years later, here's what I've learned about the differences between the two.</h2>

### Moving from low-coordination to high-coordination contexts

At Lyft and in roles prior, I worked on teams that were highly independent by design! By virtue of these companies having startup DNA and by virtue of existing during the zero-interest-rate 2010's phenomenon, we had mandates to move fast and to be decoupled from other teams.

At Lyft I was in the Growth organization which was **peripheral** to the core Ride Booking experience. I was tasked with building user acquisition funnels and retention flows, which funneled into the core product experience. But because there were very clean lines in the user journey where we could draw a boundary on, the organizations had very clean lines of separation from each other. This gave my team a very long leash to execute our big ideas on. It was taken for gospel that we needed to ship fast and iterate quickly - my director told us his goal was to have a team ship a new experiment every week.

Now switch contexts to Google (YouTube), where I work now work on a **core** YouTube product surface - one that is embedded deeply in the product experience and with countless dependencies on other teams. For one my teams, I counted no less than 12 product- and infra-team dependencies. Want to ship a feature of any significant product impact? That's how many teams need to convince, align and get approvals from.

As a result, you get heavy process- and planning-driven cultures, with technical program managers running the show (by the way, bless my TPgMs, they are a godsend and the only way anything gets done).

## Moving from microservice to monolithic architectures

Following Conway's law, it's little surprise that our architectures reflected our org structures. Lyft was on the bleeding edge of the microservices wave in the 2010s (our frontend engineering guild of ~100 engineers counted nearly as many frontend microservices total - that's nearly one service per engineer!). Want to get a change in? No problem - service boundaries cleanly match team boundaries, APIs are exposed between other teams and you can just basically write your code and ship it in 15 minutes.

Google, on the other hand, is famed for having a monolithic architecture (which is, as it is, experiencing a resurgence lately). This means that engineers are often queued up waiting for code reviews on other teams that are dependencies or owners of upstream services that interface with your feature. This is by design, though - the bar is set very high for any changes entering the codebase, and having a monolith gives engineers visiblity and accountability to maintaining its high bar. 

Note that I'm not suggesting that a monolith necessarily means more perverse coupling or more technical debt - in fact, the Google way of doing things is incredibly elegant, and there is a lot of proper thought given to encapsulation of concerns.

But taken to the extreme, this can often mean playing review tag for days at a time with a team that's on the opposite side of the globe, leading to very real implications for getting code shipped.

### Product maturity and scale dramatically dictate how you approach product development

At Lyft, we were the #2 player in the market and always playing catch-up. We were always worried about losing users and market share to the competition, and this do-or-die mindset was always at the forefront of our decisionmaking. Thus, we were hyper-focused on shipping new features (even if the bets we were taking were a little risky). Reviews were fast and decisions were made quickly because the threat was existential. The question here is: *are we shipping fast enough*?

On the other hand, at the scale of YouTube as the 900-lb gorilla in the space, *the business is designed to defend itself*. There is so much more to lose than to gain from shipping new features. Want to do something radical? Product owners spend months debating, aligning, drafting annual roadmaps, then shipping them around to get reviewed and aligned with other teams to ensure that the feature you want to build is actually the right thing to build. Vision docs are written, alignment sessions are brought in, OKRs are hotly debated for quarters on end. The question here is: *are we shipping the right thing*?

### The curse of data-driven decision making

At both of these companies you'll find experimentation-driven cultures and advanced experimentation tooling, but Big Tech takes it to a new level. At Lyft, we definitely had a few north star metrics that we aimed to move (or used as guardrails) - but we really only tracked a handful, tops.

At YouTube, there are literally thousands of metrics that can move with a change, and you had better have a good idea why they move. Any change in metrics is inspected with a microscope, any deviations can send engineers and data scientists off on data expeditions to understand what happened. It could be real, or it could just be statistical noise. And God forbid this is a metric that you have never heard of - better go find the right person to ask and figure it out.

This leads to what we've internally called "metrics hell". When a metric goes awry, how do you know:

* What actually is happening? What are our theories about why this is? Does it logically make sense to the change we are testing?
* Is it statistical noise - and can be ignored?
* What other kinds of analysis can we do to understand this outside our experimentation system? Manual testing? Deep-dive into log data?
* If it's a real metrics drop, how much can we tolerate? When can we get it back?

Teams can spend quarters working on a big project only to get it up to an experiment then get stuck in Metrics Hell for months on end. It is *very* rare to see a project get the go-ahead if a core data concern is not resolved and investigated. This means we need to deeply instrument the system with the right data inspection tools, and train our engineers to query and analyze it.

### Moving from zero-interest-rates to zero-headcount scenarios

Finally, this is maybe the most important point.

When I was at Lyft from 2019-2022, we were in a big wave of IPOs. Crypto frenzy was everywhere. Tech, coming out of the pandemic was white-hot, and 2022 felt like the year that workers held all the chips in their hands. I had never felt so much confidence in my prospects in the market.

It would soon all change as 2022 came to an end. Only a half year after my departure, I received news that my prior team at Lyft had seen heavy layoffs and more or less been disbanded. One by one, companies started to fold and layoffs started to materialize in places I had considered invulnerable to layoffs. The unspoken feeling was that we were all vulnerable, and that no job was safe.

Where in my last role there was a feeling of freedom, of experimentation and trust and *play*, the dominant experience of the next role was... *stress*. I had never felt so much pressure to onboard quicker, learn faster, work longer hours, and drive impact. It exhausted me, and work was no longer enjoyable.

So where does that leave me now? Well, I'm hoping to explore some of these learnings and themes in my next few posts over the next year. Some of the musings will be very practical about navigating a Big Tech job at Staff+. Other will be pretty personal about managing anxiety and stress. My goal is the write the Survival Guide I wish I had when I started out this new role - and hopefully it will be useful to you too.