---
layout: post
title: Escape from Metrics Hell
categories:
- Experimentation
- Engineering
date: 2026-09-06 23:15 -0700
---
<h2 class="intro">Uh-oh, you’ve been plugging away at this feature launch for months and you just can’t get the metrics to be positive enough to ship. Is your launch cooked? What to do when you just can’t get your experiment metrics to go green.</h2>

If you’re like me and you’re shipping products at scale, chances are you’re using the metrics from an experimentation system to inform your decisions on whether a feature is ready to launch.

In my day job at YouTube, experiments are running at massive scale. Not only are the user population massive but so is the experimentation surface area - nearly every single aspect of the product emits a metric and run through our experiment pipeline - think tens of thousands of metrics computed across billions of users. You can’t do the smallest thing without running it through an experiment, for better or for worse [^1]

If you’re also like me, you’re oftentimes stuck in **Metrics Hell**[^2], which is our term for certain unlucky launches that for various reasons find themselves in long cycles of poor launch metrics. It seems like no matter what you do, no matter what bugs you try to fix, there’s always some cluster of metrics somewhere that are persistently negative. The team finds itself playing whack-a-mole with launch metrics (each product may track hundreds), and this process can drag itself over days, weeks, months, and (in some unlucky cases) years.

What can we do to get out of this special hell? Below is a set of notes from the trenches.

## Prioritize the metrics that matter, and deprioritize or deprecate metrics that don’t.

Teams can get stuck if you think every metric is the same and should be weighted the same. In reality, there’s a priority ladder of metrics, and you need to get the team on the same page of what the north star metric(s) should be.

This will usually be your north star / OKR business metric. But it should also be a set of 3-4 good metrics that cover both the user experience and technical excellence.

**Try this:** spend time with the team and write down your Must Track metrics. Conversely, deprecate (or agree to fix) metrics that are not meaningful, misleading or noisy.

## Build your confidence in your data infrastructure

Some teams I’ve been on have been bitten because they only have a surface understanding of their metrics and how they are derived. For example, there may be unknown telemetry gaps between mobile platforms where there’s better coverage for Android than iOS, or the metrics mean slightly different things depending on the implementation. Or you may have coverage in one happy path flow, but it’s missing in a few edge cases. Or you may be unaware of outliers in your user data that are silently skewing your readouts.

One team I worked with didn’t have an easy way to access user logs (the organization kept this data very locked down). This friction changed how the team operated; they were more prone to guessing at hypotheses from the metrics without looking for the evidence from the logs. Once we figured out a way to make the logs more readily available, the team was able to root cause their issues far faster. They went from rolling the dice with their fixes to actually connecting dots from system data.

Or it could be latency related: long cycle times for data pipelines mean the experiment data won’t come in for five days and slowing down your team’s execution.

**Try this:** do a data spike with the team, looking for edge cases where you can be plugging some of these gaps in telemetry and logging, or making system improvements to reduce experiment.

## Understand the limitations of statistical methods

If your experimentation system is using 95% confidence intervals, then there is still a 5% chance that your metrics are wrong. That's just statistics.

**Try this:** Use an experiment sizing calculator to understand how much experiment traffic you must send your experiment to achieve statistical significance. If this is a highly critical experiment, consider upping the confidence interval threshold (sometimes we use 99% CIs) to reduce the prevalence of false CIs. You’ll need a lot more experiment traffic in these cases though.

You can also try to run an A/A test (null hypothesis test) to test if your experiment randomization (what we call diversion) are truly random. This will also show you what metrics are prone to noise or are of high variance.

Finally, beware of metrics that are easily skewable. These may be metrics whose values are unbounded toward infinity, and who can be skewed by that one user or one flow that is bonkers beyond your assumption of how your Normal User behaviors.
(You may prefer to derive a metric that measures median or p95/p99 rather than a sum or average).

## Narrate the launch as a long-term strategic bet and commit to following up later

This is more of a framing tactic to use with leadership. If you feel like the team has given it their best shot and the metrics continue to glow red, then it may be possible to change the frame of the launch as net-positive to the business to a frame of an investment - we take the hit now to generate opportunities later.

What is *not* going to work is if you go to leadership without a clue as to why they are still negative. Make sure you trust your data and understand the metrics first (see above)!

**Try this:** If you choose this tactic, you may want to set up a long-term holdback experiment to continue having the ability to fix the underlying issues without the launch pressure.

## Experimentation and metrics may not be for you

Running experiments at a Big Tech (or Big Tech adjacent) company? Table stakes. Must-have. Ingrained in the culture.

Running experiments for a startup with 100 users? Probably not necessary! You’re probably better off talking directly to your customers doing interviews. Your experiments will likely be high noise and subject to change as your user base fluctuates. Ditch the experimentation entirely (or read them out with a grain of salt).

## If all else fails... kill the project

This one is hard to swallow, but the sunk cost fallacy is real. If you’ve struggled this far without any breakthroughs and you’re unable to convince leadership of the strategic importance of the project - it may be time to cut your losses and move on. 

It’s not easy to take the L, but I find that worse than this is if you let the bleeding continue. It’s painful for all involved. Closure is good, and frees up your team for new opportunities.

[^1]: OK maybe you can fix a typo, or add a few loggers here and there, but truly nearly *everything* goes through an experiment.
[^2]: Product leaders wiser than me coined this term and drafted an internal memo similar to what I’m discussing here. The ideas there really stuck with me - I consider it one of the most important documents I’ve read at my current employer.
