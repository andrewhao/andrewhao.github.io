---
layout: post
title: I choose XP
date: 2018-09-04 17:13 -0700
comments: true
categories:
- Agile
- XP
- Process
- Engineering
---
<h2 class="intro">A discussion about why I enjoy working in the XP-school of Agile. (tl;dr: I enjoy working in a pull-based work stream. I like that pair programming, TDD and other best-practice batteries are all included.)</h2>

Several articles have been circulating recently in the Agile community critiquing the current state of the practice (see: ["The State of Agile Software in 2018" - Fowler](https://martinfowler.com/articles/agile-aus-2018.html) and ["Developer Should Abandon Scrum" - Jeffries](https://ronjeffries.com/articles/018-01ff/abandon-1/)). At their core, they are really saying the same thing - Agile, as originally intended, was meant to be a living, breathing, _freeing_ process for the team. Compare that to the rigorous imposed frameworks deployed across organizations these days.

### Agile Scrum: A story

Long ago<sup>[1]</sup>, I worked at a company that deployed Scrum across the entire company. This wasn't some mega-corporate behemoth, it was a mid-stage startup that had been around for a good while. But it was struggling. Teams across the org weren't working the same way. It was hard to pin down milestones and delivery dates and coordinate work the same way. Some engineering teams worked in an ad-hoc, Kanban-ish way. Others were more centralized, top-down planning. Project managers wanted more predictibility in our delivery platforms.

And lo, an Agile working group was formed, and they chose to roll out Scrum. And to be honest, the implementation of Scrum we chose wasn't a bad one. A process was rolled out in which we all used JIRA, had regular planning meetings, standups, retrospectives and all that jazz. But something still wasn't sitting well with me.

We assigned stories to engineers at our planning meetings. "Oh, Jay is good at the payments system, so he'll take this story". Jay mumbles something in agreement. "Esther knows infrastructure items better, so she'll handle the deployment tasks on this task." And Esther would do it. But pre-assigning stories would lead to knowledge silos, and creating these false boundaries of what was "acceptable" to work on.

Our project manager - bless her soul - would attempt to answer big questions through Scrum in a way that abused it. We were asked to estimate stories three sprints out so a delivery date could be given to upper management and other stakeholders. Of course, we would always miss these targets. (Once, we were asked to estimate the scope of an entire project by interring the entire team in a room and writing out six months' worth of stories!)

It was clear that Scrum had brought some wins. Retrospectives really were introducing a feeling of continual improvement and shared accomplishment. Estimating stories was helpful in raising conversations to the fore that would help the team arrive at an accurate estimate. But there were still some pain points - Scrum was still leveraged by our project management structure to organize major efforts toward milestones and deadlines in ways that were pressure-filled and potentially harmful to the team. In short - Agile process was implemented in a top-down manner that emphasized control, predictability and rigidity. And it didn't feel great<sup>[2]</sup>.

### A better way?

Around the same time, I started attending an XP meetup (hosted by Pivotal Labs SF) and I met a ton of lovely folks who were happy to inform me that, yes, there was a better way to Agile. And in fact, the better way to Agile was to embrace uncertainty and chaos more, to trust teams to do their own thing, and to bake some solid technical practices in with the work, too.

I was sold, and I ascertained that my next gig was going to be at a company that practiced XP. And so when the opportunity arose to work at [Carbon Five](https://www.carbonfive.com), my current employer, I jumped.

### How I choose to work

In my software consulting life at Carbon Five, I work on projects that ramp up quickly in domains fraught with uncertainty and organizations (oftentimes) wrestling with dysfunction. We practice what I call "lower-case xp" in nearly all of our projects. Meaning we don't force our clients to do ALL the things in [by-the-book XP](https://www.amazon.com/Extreme-Programming-Explained-Embrace-Change/dp/0321278658), but we keep the important things. Those things are:

* __Frequent pair-programming__: On stories and tasks that are fraught with unknowns, or for a team with knowledge silos, there's really nothing better than pairing through and sharing knowledge. This has been the number one way that I know how to level up developers on a team. My teams don't typically pair 100% of the team - the ratio has been closer to 40% to 60% of the time.

* __Pull-based work streams__: Since we pair program so much, the entire team tends to be very balanced in terms of their knowledge and familiarity of the system. This means nearly anyone on the team should be able to take on any task in the backlog - or find a pair partner who could. This eliminates the need to pre-assign work at the beginning of each iteration.

* __Cutting scope, pushing deadlines, but never overworking__: The team never works more than 8-hour days. We put in focused 8 hours of work, then we're off. If a project expands in scope due to unforeseen circumstances, technical debt, scope creep or whatnot, then our product owner has a choice to cut scope to meet the deadline, or move the deadline itself.

* __Short iterations__: I have really enjoyed one-week iterations. Two is fine. Anything longer than that, and it feels like a slog.

* __Craftsmanship practices - batteries included__: XP isn't particularly prescriptive, but it does recommend development practices like CI/CD, and test-driven development. I find that folks who buy into XP also tend to be a certain type of programmer that values software design and testability<sup>[3]</sup>.

* __Continual improvement__: This is arguably the one thread that, no matter what Agile religion you follow, you should always prioritize. In XP, this would be our reflection (or retrospective), where the team has a space to reflect on what went well and what could be better, then make commitments to improve upon those things. There's nothing more empowering than a team that continually improves iteration over iteration again.

### To recap:

There are lots of ways to Do Agile. My intention isn't to bash Scrum, although I've seen it abused. Matter of fact, though, is that doing Scrum can lead to lots of wins, because the principles it sits on are still solid.

My preferred method of working is with a lightweight lowercase-xp style of work, that promotes collaboration, bakes in best development practices, and embraces uncertainty.

What about you?

<small>
[1] It wasn't _that_ long ago.
[2] Some would say this was a mis-execution of Scrum. I don't disagree - I actually like the core tenets of Scrum and believe it works well when it's understood to not be a silver bullet.
[3] This is purely my observation, and is not meant to be a generalization!
</small>
