---
layout: post
title: When Resources Are Rare
categories:
- Engineering
- Big Tech
- Efficiency
---

<h2 class="intro">At this point, the global compute resource crunch has come for us all, from Big Tech to the average consumer. How do we run organizations that operate in a world of finite and constrained resources?</h2>

You’ve probably seen or heard the news - there just isn’t enough computing hardware in the world. You might have seen the price of your latest phone go up, or held off on an upgrade to your computer. Whether that resource is RAM, or GPU / CPU, or disk - it’s all being devoured by the AI computing wave that’s been building in the last couple of years. 
This is evident even to deep-pocketed hyperscalers, armed with oodles of cash on hand. There simply isn’t enough supply to go around to feed the demand in the years to come, no matter how many dollars they want to throw at the problem.

This is likely bad news for your organization. For years, we assumed that cloud providers would continue to scale and Moore’s law would continue to pay dividends - resulting in a race to the bottom as cloud providers competed based on unit economics. It seems that this year in particular, we’re seeing prices go up and global forecasts are not looking good.

In other words, we need to *do more with less*. But how?[^1]


## Governance and guardrails

Your organization may have some sort of way to hold groups accountable to resource usage. You may have a centralized resource group whose job is to track and control infra spend. In my experience, groups like this are responsible for cost projections, along with an accountability structure, much as an organization tracks its P&Ls. 

Disclaimer: I don’t have experience forming such oversight structures, so I won’t claim to understand it. But you need some sort of **planning group** responsible for developing forecasts, and you need some sort of **governance structure** to keep groups accountable to their spend in real time. 

**Each team must be able to understand its spend.** This sounds easy, but in practice is very hard to do. This will mean that this resource oversight group should be responsible for making infra costs easily visible (dashboards, data tables) and attributable directly to teams. You may need to set up the proper user groups in your infrastructure to properly be able to map services to teams. Some shared services may also need finer-grained instrumentation to understand how their cost attribution works. This is enough to fill a book about - it’s out of scope for this article!

In my experience, there are two ways to do this - top-down and bottom up.

## Top-down planning for a resource-constrained world

The most direct way to control cost is to **set a top-down target**. This seems self-evident, but painful. The units of this target will vary. They may be direct financial costs (dollars, euros, etc), or they may be aggregated to virtual units of hardware or software cost.

How do you set a top-down target? You  first must have a **growth projection:**

* Each group can assemble its own growth projections for the next period, based on expected new launches or on organic growth.
* You can simply take a trend line forecast and estimate your growth into the following year
* You can average the predictions of multiple forecast models.

From this projection, you can set a target *under* the projected growth estimate that meets your financial capabilities. You can turn around and give each organization or sub-group their own efficiency targets - constrained growth targets for the upcoming period. You can choose to prioritize one organizational or business function higher than another, or ask everyone to contribute equally. That’s a matter for you to decide.

## Not all workloads have the same ROI

Not all workloads are the same. For example, we may find that our CI systems are taking up 90% of our EC2 cost, whereas production server costs are the other 10%. In another analogy, you may find that 20% of your workers are burning 80% of the tokens. 

Are each of these cohorts capturing the same amount of business value? Well, it depends! In an ideal world, we could compute some sort of ROI metric to understand their relative importance to the business as a way to develop guidelines.

For example: let’s look at our total bill for an AWS service, say AWS EC2. Let’s say that some amount of it is provisioned for developer sandboxes, some amount for our internal CI tooling and developer infra, and some amount of it is for production server traffic. As we said, the ROI for the production servers is likely higher than the ROI for the developer sandboxes. This might suggest that we may ask the Developer Infra teams to look into local tooling, or some optimizations that reduce cloud spend.

Or in the AI world, we may find that 1% of our workers are disproportionately spending the majority of the tokens. The ROI of each of those incremental tokens may be less than we expect. (Famously, the jury’s still out on ROI on AI spend, so good luck).

If we’re a SaaS business, we may find that a subset of our customers are voracious consumers of infrastructure cost, but that spend is generating very little customer value (or business value).

In my day job, we create a proxy metric for **business value**, then attribute the resource costs to drive that metric. We then come up with the unit cost metric of business value - a ratio metric.

Unit Cost = Cost / Business Value

This opened up a whole slew of ways to look at our product and disaggregate the product from a unit cost lens.

## Making decisions

Here’s the hard part. Someone is going to have to make the call on how to prioritize the relative ROI on each. This may have real impact on product or infrastructure.

* You may decide that one segment of customers is no longer worth going after, and shift to prioritize on a higher-value cohort.
* You may decide to sunset a feature that is costly and not delivering business value.
* You may ask one part of the org to save more than another part of the org, simply based on business outcomes.

None of these are easy decisions, and they can all involve tradeoffs. Ensure you’re aligned with your leadership team.

Once you do so, you can then roll up **cost targets** in accordance to your prioritization to each group and ask each group to save accordingly.

*This post has gotten a little long, in Part 2, we’ll go over bottom-up ways to save + become more efficient!*

[^1]: I’m writing from my personal experience working with central resource management teams within a Big Tech company. I don’t speak for my company; views are my own and all that jazz.


