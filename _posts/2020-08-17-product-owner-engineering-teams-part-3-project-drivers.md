---
layout: post
title: 'The Product Owner Engineer - Engineers as Project Drivers'
date: 2020-08-17 21:13 -0700
comments: true
categories:
- The Product Owner Engineer
- Product Management
- Engineering
---

<h2 class="intro">When individual empowerment is your radical idea, then you commit fully to it with the Engineer as Project Driver. All engineers, no matter their experience, are given the responsibility to take a project from start to finish and own the outcomes.</h2>

In our [last installment, we saw how the Idea Backlog was a great tool]({% post_url 2020-06-23-product-owner-engineering-teams-part-2-the-idea-backlog %}) to generate new ideas. Now that we've got great, well-defined ideas with clear measurement metrics and juice opportunity sizes, how do we execute on this work with the team?

## What is a Project Driver?

The **project driver is the individual responsible for the outcome of an experiment or project**.

The driver may do things like:

- Be the point person to loop in additional resources like Design, Data Science, or QA.
- Write the technical spec describing the feature or experiment to launch.
- Do the market research to understand the opportunity size or impact analysis of the change of this feature (ideally quantifiable to a core business metric - lift metric X by Y%, increase revenue by $Z).
- Collaborate with adjacent teams to hammer out expectations or give advance notice of the expected changes their changes will drive.
- Send out announcement and update emails explaining the launch of the feature, as well as follow-up emails reporting the results of the launch.

The driver does *not*:

- Write all the code alone. We encourage teammates to pair with each other regularly or pitch in on parts of the project, especially those with multi-iteration timelines.

## The Project & the Project Driver

We conceptualize the lifecycle of a Project from a Project Driver's perspective into **Plan**, **Build** and **Measure** phases:

![Plan-Build-Measure diagram](/images/product-owner-engineering-teams/plan-build-measure.png)

### Plan

In the **Plan** phase, the requirements are still being gathered and relationships are being built. This is the "soft" part of the project - we are trying to convince folks to help us or commit to collaborating with us in this form.

A tech spec may be written and circulated among relevant teammates. Clear experiment hypotheses or measurements for success are defined and documented.

### Build

In the **Build** phase, we actually build the systems and write the code that support our feature. There is a project management angle to this as well - requirements are written as user stories and logged into JIRA or system of choice.

The team (or teams) build against this backlog.

### Measure

In the **Measure** phase, we launch the product and we let it bake, gathering metrics along the way. The product should ideally be launched in a manner that facilitates A/B testing so we can accurately measure the effect of the change.

Once we release our change and enough time has passed, we report our results to relevant stakeholders and mark our hypotheses as **Validated** (or not). We then turn around and use these learnings for a second iteration of our hypotheses and return into the **Build** phase to focus in some more.

Note that not all experiments or product launches are destined to live another day. Some should be reverted or axed. Others will live on as future product ideas or learnings to apply to a different domain.

(Astute observers may see that this process is a rebranded version of the Lean Startup **Build-Measure-Learn** triangle.)

## Everybody should be a Project Driver

In skills-diverse teams, there will be a natural gap between engineers with different amounts of industry experience. We believe that all engineers, even new grads, can be trained to drive projects of increasing scope.

### ...the New Grad

Let's take the **New Grad** as an example:

Although this is his first job out of college, our new grad is given a simple project to drive - an A/B testing experiment that adds a new module to a landing page.

Even though this project's **scope** and **complexity** is small, our new grad will be able to pick up some incredible experience from it:

* He will learn to communicate with adjacent teams when the changes require a new API change from an upstream service maintained by a sibling team.
* He will learn to write queries in the backend when building out an analytics dashboard measuring the impact of his changes. By writing queries, he also gets closer with actual customer behavior - maybe here he discovers a hidden cohort of users that can be targeted differently.

The **New Grad** will succeed as a Project Driver if:

* We set him up with a more senior pair buddy who can walk him through the steps and in real-time, mentor him.
* Expectations are clearly set and documentation is plenty about how to perform the Project Driver role.
* The team exhibits psychological safety around failure.

### ...the Experienced Engineer

On the other end of the spectrum, let's take our **Experienced Engineer** as another example:

Even though our engineer is highly skilled, her growth arc will increase when she is challenged to ideate at a size and scale larger than she's taken on before by owning big ideas with big outcomes. Here, her project is the development of a new machine learning platform that personalizes the search experience for the product.

* She will be asked to consider leveraging new technologies to apply to new problems - is there a way to apply statistical models or machine learning to this domain?
* She may pitch a Really Big Idea that captures momentum and require the collaboration of multiple teams - of which she is the technical lead. Here, she may be asked to involve contributors from the ML engineering group, as well as contributors from the realtime event streaming platform alongside her Search Product team.

The **Experienced Engineer** will succeed as a Project Driver if:

* She is able to relinquish some coding responsibilities (even the juicy ones) to the team.
* She is able to stay in the realm of project leading and technical design, building good feedback loops with other PMs and tech leads.

## Final note - team coordination

While everyone has the opportunity to be a project driver, not every team member should be individually driving a project all the time. Projects should be sequenced so that teams do not have more than a handful of active projects at a time (limited WIP).

We encourage engineers to lead for some season, but in other seasons they can be the supporting cast for another engineer's project. This allows all of our team members to be in the driver's seat. Pun intended.

*In our [next installment, Part 4]({% post_url 2020-08-17-product-owner-engineering-teams-part-4-the-four-hats %}), we'll look at how it all comes together by rotating our team through the four "Hats" of Product Management. Tune in!*