---
layout: post
title: 'The Product Owner Engineer - The Idea Backlog'
date: 2020-06-23 22:10 -0700
description: How do you teach engineers to think like PMs? You give them the awesome and scary responsibility of choosing their work. Our team learned how to think and prioritize strategically by collectively building an Idea Backlog where we dream up and prioritize impactful projects to tackle. Here's how it works.
comments: true
image: /images/product-owner-engineering-teams/idea-backlog.jpg
categories:
- The Product Owner Engineer
- Product Management
- Engineering
- OKRs
---

<h2 class="intro">How do you teach engineers to think like PMs? You give them the awesome and scary responsibility of choosing their work. Our team learned how to think and prioritize strategically by collectively building an Idea Backlog where we dream up and prioritize impactful projects to tackle. Here's how it works.</h2>

## What if we turned all our engineers into product owners?

In [Part 1]({% post_url 2020-05-11-product-owner-engineering-teams-part-1 %}), I told a story about how the departure of our PM led my manager and I to split the PM roles between ourselves, to shield the team from the change. This spread the two of us way too thin, which also caused us to be less effective overall. That clearly wasn't going to work, and burnout lay around the corner.

We decided to make a major change to the team structure with a process that would focus on individual empowerment. What if all our engineers were empowered - and _required_ - to decide what to work on in a truly radical way? What if we asked them all to think like product owners, and take responsibility for team output?

Instead of being fed work from a product manager, our engineers would be responsible for thinking of new features and projects that drive growth and seeing them through to completion through the entire software development cycle. Terrifying? _Absolutely._

## Individual Empowerment through the Idea Backlog

![Illustration of two figures putting ideas onto a backlog](/images/product-owner-engineering-teams/idea-backlog.jpg)

In this new model, the ball starts with each individual contributor. Armed with a clear vision of the goal (OKR) and the tools to measure them (metrics and analytics), the team is given leeway and authority to work on any project that can drive impact.

We track this process with an **Idea Backlog**, a prioritized backlog of ideas that are in various stages of maturity. Ideas may range from:

* Landing page experiments involving copy or UX tweaks
* New partnership ideas with other companies
* New product ideas that solve a specific problem in the customer journey
* New ways to gather feedback from users
* Total moonshots - 10-year scenarios!

We ask teammates to continuously brainstorm and generate new ideas to keep the creative cycle going. Each week, we check in with the team and see if people have any new ideas for projects and experiments that can move the needle.

## How do you think of an idea? Get people in front of customers.

People don't just think of ideas unprompted! **One way we help the team think up ideas is to get them consistently in front of customers.** For example, by manning a chat widget on one of our landing pages, our team had to talk to several customers a day and learn about what they were having trouble with - those conversations led to product ideas that eventually became experiments!

In another experiment, I ended up piloting a customer-facing product aimed at restaurant owners and other SMBs. I had long phone conversations with restaurant owners and discovered new use cases and concerns that I had never known about. These become ideas as well.

Other ways we do this is by chatting with our UX researchers and reading customer interviews done by others. No matter whether your company is 5 people or 5000, there are always ways to break through the inertia and get the team in front of customers to build empathy.


## Idea Backlog: Product Spec 1-pagers

Our **Idea Backlog** is really just a Kanban board of big ideas; rough and unfiltered. But they need to eventually get vetted. We do this in a weekly check-in where we vet the fitness of each idea.

Each idea backlog item moves from "this is a cool idea" verbal discussion into written form as a 1-pager specification, written by the idea's author. This specification needs to have:

- A sense of the opportunity size or impact, usually measured in one of our key metrics ($ or another core business metric - rides, in our case).
- A discussion of how the implementation will work, usually defined as a A/B experiment that is measurable and time-bound.
- A discussion of the risks and non-goals involved.
- Any external stakeholders or teams that also need to be involved.

Yes, it's painstaking work that often lives outside of most engineers' comfort zones. This means oftentimes setting up meetings with different teams, other PMs, and product leaders. This means writing *specs* (ugh). This means getting down and dirty with data analysis, writing queries and hunting down data tables that may be poorly documented or hard to understand.

In other words, this isn't *coding*, but it's a foundational part of the exploratory analysis needed to be a product owner. And we have our engineers all go build that muscle.

This is also time-consuming - idea generation should be factored into team sprints. It's not uncommon to spend a whole day writing queries digging through data to form a hypothesis, or finalizing the 1-pager spec that is to be circulated.

## How Ideas move into the backlog

At the beginning of each week, we evaluate our roadmap and see if there is any need to "pull" a new idea from our idea backlog into the roadmap. At this point, the team can decide together whether an idea has enough legs, definition, and impact alignment so as to actually become a formal work project.

The Ideas that rise to the top are the ones that have at least one of these traits:

* Has a high impact-to-effort ratio
* Has a total opportunity that meaningfully moves the needle
* Helps us learn something we don't know and could validate a new market segment, partnership, or use case.

Thoughtfully designed Ideas will also be:

- Defined well enough so that they are actionable to one or two weeks' of effort.
- Backed by clear metrics (or are designed with the right data instrumentation in mind to be able to retrieve clear metrics)
- Launchable easily with minimal dependencies

This usually means that the kinds of ideas that get wings are quick tests - for example, we want to test the listing on the App Store. Or we want to test a quick JSON-LD rich snippet change on a web page that might cause our Google search index rank to rise. Or we deploy an alpha prototype to a small group of trusted prerelease customers.

It doesn't mean we avoid ambitious, multimonth releases, but those are vetted with more deliberation and care, and do require much more unblocking and team alignment. My manager counterpart will often have to do some roadmap planning and stakeholder communications with upstream/downstream teams to get buy-in consensus for the very big projects.

## Measuring success

And finally, the hallmark of an idea is that there are clear metrics for success - or failure. From this, we take a page from the playbook of the [Lean Startup](https://theleanstartup.com/principles) manual. We oftentimes use an [Experiment Canvas](https://designabetterbusiness.com/2017/11/30/experiment-with-your-riskiest-assumption) to help think through our experiment design to build metrics that are clearly quantifiable and time-bound.

Features are launched with metrics dashboards built as part of the feature work. Clear metrics are essential to our ideas since they force us to see the outcome of our work through actual data, give us the courage to roll back changes that are ineffective.

## Some caveats

Interested in trying out this method? Of course there are limitations, and here are some of them:

* Make sure you have buy-in from other product organizations.
* As you might imagine, bottom-up ideation must be guided by well-defined OKRs. We are fortunate to have OKRs that are well-designed and aligned with the rest of the company.
* Having an Idea Backlog doesn't mean your team doesn't have long-term strategy. You still need someone to think about the long-term bets and plans for the team (skip ahead to [Part 4]({% post_url 2020-08-17-product-owner-engineering-teams-part-4-the-four-hats %}) to read more about how we do that)

## Idea Backlogs - a great way to get involvement from  your teams

The projects that make it out of this cycle get formalized in our roadmap and worked on in upcoming sprints. Better yet - all engineers are responsible for the generation of these ideas, so the sense of ownership they feel over their idea is huge.

*In [Part 3]({% post_url 2020-08-17-product-owner-engineering-teams-part-3-project-drivers %}), we'll discuss how we distribute this work among team members and got the day-to-day work of executing and strategy development rolling among the team!*
