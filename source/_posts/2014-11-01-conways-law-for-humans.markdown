---
layout: post
title: "Conway's Law for humans"
date: 2014-11-01 18:38
comments: true
categories: 
- conway's law
- engineering
- management theory
- management
---

If you're familiar with [Conway's Law](http://en.wikipedia.org/wiki/Conway's_law), it states:

> Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure.

Or in layman's terms, your software systems reflect the structure of the teams that create them.

Think about it -- do your teams prefer to do everything themselves? Or do they ask for help from other teams? In general, as a team, we prefer to have as little dependencies as possible. The work that the engineer has to do to send an email, or wait for work from another team (like an API change or design change) is usually time-consuming and burdensome. Therefore, it is not (usually) in the team's best interests to cross silos and ask for help from others. Your teams, if they look like this, tend to work in codebases that are generally monolithic and wholly owned by the team.

It's not wrong, or it's not bad, it's just a sociological observation. This is why companies like Spotify, Netflix or Amazon have embraced Conway's Law and changed their organizations to match the  microservice-driven architecture they want. Small teams work on small codebases and are empowered to make the changes they need.

### A corollary and some observations about your company culture.

Here's a corollary, which I've heard in various shapes and forms. Paraphrased:

> An organization's structure tends to pattern after its leaders' communication patterns.

I've been pushing an effort to unify our company's frontend styles and UX into a unified framework in an attempt to standardize the look and feel of the site.

However, in working with our designers, I realized that they weren't talking to each other. Designers in one department had opposing design aesthetics from designers in another. This was causing problems in the code, because the frontend framework itself was becoming fragmented. You could see it in the code. Version A of the styleguide went into Department A's products. Version B of the styleguide went to Department B's products.

In this case, as we kept rolling out this framework, I realized our organization had no single owner for the design language of the site. Why? I had to wonder if it had to do with some deeper communication issues between the heads of the two departments.

Code can be a good canary to organizational issues, and call out larger human issues at hand. In this case, Conway's Law helps us root out and bring into the light structural concerns. Leaders can pay attention to these concerns and check themselves to become more open communicators.