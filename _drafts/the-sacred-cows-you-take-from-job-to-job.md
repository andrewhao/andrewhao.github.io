---
layout: post
title: 'How Now, Sacred Cow?'
---

A friend had introduced me to a peer from another company last year who was having troubles adjusting to his new role as tech lead on his new team. We sat down at a local coffee shop to talk it through.

"My team just doesn't know how to do Agile" the engineer stated. "I propose all these process changes but people are  skeptical."

Having myself been hired by my employer as a team lead, I knew how frustrating that could feel. I pressed on with some more questions about why the process was off.

"They're pre-assigning stories at the start of the sprint. They estimate with time instead of points. And they're all working in individual silos" this engineer sighed.

There was one last question I had to ask - "Is there a specific problem that's happening as a result of this broken process? Is anything actually *wrong*?"

There was silence as my conversation partner thought further. "Not really," he allowed. "They seem to be working out all right."

"Does your manager think anything's wrong?" I asked.

"Hm, not that I think about it. He's understanding of what I want to accomplish, but the team's been together for a long time and this is just how they work. People are willing to make the changes, but they're skeptical what benefit it would bring," he acknowledged.

### Is there actually a problem?

Kent Beck, [in an interview on the Software Engineering Daily podcast](https://softwareengineeringdaily.com/2019/08/28/facebook-engineering-process-with-kent-beck/) describes coming in to Facebook in 2011 and having his fundamental assumptions about software engineering challenged (for those of us who are unfamiliar, Kent Beck is one of the original authors of the Agile Manifesto and earliest advocates of XP and TDD):

> Interviewer: When you joined Facebook, my understanding is that around that time, Facebook really didn't have much testing. It's ironic, because you were the creator of extreme programming. It was highly dependent on the process of writing unit tests and then writing the features. Facebook... was able to be successful, despite the fact that they wrote their features before they wrote their tests.

> Kent Beck: The answer that I came to is that... one is, how many of your problems can you test for and how many problems only show up in production? ... If you can't write unit tests for it... The Facebook answer is don't.

> The second part of the answer is tests are a form of feedback and Facebook engineers had many, many other forms of feedback [describes rollout process].

> Then the tests that I had written broke almost immediately. They were deleted. That was one of
the things that surprised me. If you had a test and it failed, but the site was up, they just delete
the test... If you eliminate this noise production, per definition the situation is clearer all of a
sudden. 

Mr. Beck realized that, despite being the very *author* of the book that espouses best practice, test-driven software development, his perspective was limited at best and he needed to recalibrate to the context of his new company.

### When the playbook doesn't apply any more

Many times we take our belief systems and playbooks with us as we progress in our careers. Who has experienced being under incoming senior leadership, having seen success at Company X attempt to implement that same process at new Company Y and see it fizzle out?

Why would that happen? After all, weren't they hired to replicate their success at their prior position in the new position?

When I joined my current employer two years ago, I had what I thought were a list of unbreakable "rules to work by" and held conceptions of what were best practices. In discussions with my to-be-manager in the hiring process, I was brought on with a mandate to uplevel the team. And coming from a background in consulting with Very Successful Outcomes™, I had a very specific set of practices and dogmas that should Always Be Followed. Things like:

* Pull-based Kansan-like flows > pre-assigned work
* Pairing as much as possible > working in silos
* Test-Driven > Test-After > No tests
* Timelines and schedules should be deferred or delayed as long as possible, instead referring to team velocity as a proxy for progress.

In my first few weeks on the team, I could already see that we were violating every single one of my principles. Work was pre-assigned. Engineers took individual projects. Tests were sparse. The PM asked (in my opinion, pressured!) individual engineers for project dates on a regular basis. Points were individually given and tied to a time scale. Egads!

I could have a talk with my manager and have the *Hey, I'm here to change everything up* talk. But I knew it was wiser to wait and observe some more. And sure enough, I was surprised.

I had assumed that it was always advantageous to treat the team as a similarly-skilled set of work executors to be able to build work in the style of XP or Kanban. I felt allergic to the idea that the team might keep individual ownership over a particular project or system. In my mind, that meant a low bus factor.

However, as I observed the team, I realized that ownership and doing the implementation work were not the same. On a team that is gelled and high-performing, an "owner" of a project may also invite other teammates to work with them on it. Ownership meant accountability and leadership, not sole execution.

In another example, I pushed back hard against any marketer or PM who would ask me for date updates. "So when's Project X going to launch?" or when writing a tech spec, being required to fill in expected launch time frames. I swallowed my pride and, after much discussion, deliberation (and padding) with the team, came up with something we could live with.

To my surprise, giving dates didn't kill me. Dates, at this company, were used as sight lines rather than cudgels. If a project slipped, then it was communicated early and everybody adjusted! I had failed to take into account that my prior experience with deadline-driven development was an unhealthy one, and I needed to recalibrate my experiences with this new team.

### Leave your assumptions at the door

There's a reason so many management gurus espouse going on [listening tours](https://hbr.org/tip/2017/06/new-managers-take-a-listening-tour-to-understand-your-companys-strategy) for new leaders before starting to execute. They need context, but they also need to see their new teams with fresh eyes.

I'll leave with this excerpt I found fascinating from an interview with Nick Caldwell (VP Eng Reddit) who [talks about making the shift from his time at Microsoft to a small team at Reddit](https://www.intercom.com/blog/podcasts/reddits-nick-caldwell-engineering-leadership/):

> Nick: Your natural assumption is to take whatever works in your previous roles and use it as a template... I view process more as a set of tools I carry around with me... you need to spend the first couple weeks just listening to the problems people on the team present to you. Then, you can dig around in your process tool bag, figure out the right tool for the job, and adapt that tool to fit the situation.

What tools, processes, or sacred cows do you bring along with you, and is it time to re-examine those?
