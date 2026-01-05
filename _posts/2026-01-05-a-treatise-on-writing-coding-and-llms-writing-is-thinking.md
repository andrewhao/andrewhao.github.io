---
layout: post
title: 'A treatise on writing, coding and LLMs: writing is thinking'
date: 2026-01-04 23:00 -0800
categories:
- Machine Learning
- AI Coding
- Thoughts
---

<h2 class="intro">Steve Jobs famously said that computers would be like "bicycles for the mind". What effect will AI have for our minds?</h2>

Steve Jobs was remarking on the fact that computers would augment human abilities and allow humans to accomplish far more than they ever would have without. Now, with the dawn of the AI age, it would appear like we are about to make another leap in human capabilities. Or are we?

This last year I really jumped deep into the use of generative language and coding models in my work and personal life. Have I been seeing the payoff commesurate to the hype? Both yes and no - but this post will first start off with the pessimistic case.

### AI generates hollow artifacts that can deceive

AI at this stage is impressively good at generating code. It's impressively good at generating code that looks plausible and passable for real, thoughtful work. But looking like the real thing does not mean it is of the same caliber as a human. Why?

I've been trying to get to the bottom of this feeling; the feeling that after working with an AI system for any period of time that the work being generated is not *intentional*. Don't get me wrong, the quality of the artifacts being generated - functional code, tests, and documentation - is fairly high quality. But it's deceiving - peer at the code, and the system is doing things that look correct but fail in non-obvious ways. The code *looks* correct. It has all the right documentation. The test suite *seems* complete. But peer closer:

* New patterns and data structures are invented and injected when compliance with existing patterns would be preferred.
* The test suite blindly writes test cases that either don't need to be written or are overly verbose.
* Blind adherence to application or codebase prompts ("ALWAYS follow TDD" or "You must always write a feature spec" leads to code bloat and reduced long term maintainability)

### AI's strengths (broad) and weaknesses (shallow) create distinctly unique artifacts that are unlike anything we've seen.

Now you might say, "hey, this is just the thing where an AI is like that overeager junior developer." But it's a wildly different beast. A junior developer usually has a clear limit of where she or he can operate. Maybe are able to implement some key feature within a system boundary, but a single misunderstanding of fundamental system assumption leads their solution to be nonoptimal or incorrect. That's really easy to code review, or sit down and correct.

But an AI, they're that intern but with *wildly overconfident estimation of their abilities* and a truly dizzying breadth of knowledge. They literally have the entire corpus of the Internet downloaded into the foundation model, with all of the patterns and knowledge. **This makes correction and code review exhausting**:

* AI models often overcorrect or overimplement in subtle ways. 
* Sycophancy problems mean that AI models have difficulty pushing back meaningfully.
* With their vast knowledge, AI output often incorporates new or external concepts that do not quite fit the codebase. Depending on your intent, this could be a feature (hey, this new approach works better) or a bug (hey, this new approach is totally overbuilt)

### AIs need consistent humans in the loop to correct or justify their approach. For humans, this is exhausting.

When Claude Code started to ask you "hey, is this what you mean?" and ask you questions to get you to clarify your intent, this was pretty amazing. It meant that the context that lived in my head, the implicit understanding that I would have never dumped out into the open was being pulled from me.

But on the other hand, it's really exhausting and explains why long term using AI has not yet translated into wild productivity gains for myself. Having an AI continually prompt you for clarification on every micro-decision used to be something a programmer *just did* implicitly in their head; one of a million micro-decisions that formed the direction, vision and principles of a codebase. Having that pulled out into the open is not just wasting tokens, but wasting time.

### Human writing (and human coding) is thinking, which is valuable in and of itself

My final and most important point; by delegating this work to AI, we lose out on the reward of reasoning and learning through thinking and work. I feel this is wildly overlooked by the AI conversation today, with most commentary along the lines of "work is now all about having product taste" or "programming is now becoming a higher level activity" as if humans just needed another layer of abstraction. 

But the dark side of abstraction is ignorance, either knowingly or inadvertently. The foreman of a group of workers is no longer intimately connected to the craft. The manager of a team of coders is, purely by their (lack) of exposure to the direct work, less and less connected to the system. Our inability to interrogate an AI to understand its reasoning and the prolonged exposure to this new level of indirection is whittling away our ability to reason about and assure ourselves of the system's integrity.

I've been working on a large project in a personal codebase of mine. Day by day I've been using AI to build things, carefully crafting CLAUDE.md instructions, reading best practices for context engineering, and dutifully doing my part to keep the intention of software clear, precise and defined in text. I'd be lying if I said it wasn't satisfying; it's so cool to see commit after commit land, sprinting from one checkpoint to another. I'm burning down the backlog at a rapid rate. I've started coding on my phone, managing my AI agent directly while running errands at the store. So it's undoubtedly a satisfying experience for me.

On the other hand, it's been taxing to manage this agent every few minutes. To directly manage its direction, provide additional context, and verify behavior. I've caught it duplicating code that existed already, or writing test cases that looked correct, but did not actually test anything of value. Repeat this hundreds of times over and it begins to dawn on you: *I could have done this faster myself*.

I was with a group of friends recently discussing our work with AI and one had a very astute observation: "I think working with AI is just less satisfying". What he meant is that the distance from the work, and the work required to harness and manage an AI coding assistant was far more circuitous, far more taxing than just jumping in and doing the work himself.

I would agree. With increased use of these models, I feel the distance from myself to the produced artifact become dull. As a software craftsperson (professionally, I came of age in the XP and Software Craftsmanship movement), this pains me greatly. I am less connected, more cognitively taxed, and less satisfied with the work that is being done. As a writer, I relish the process of writing, refining an idea and polishing the gem. I love that the process of getting something down on paper (or a screen) is a dialog to refine the idea in your head, and back and forth it goes. **That** is the inherently creative part of the process I fear that I am losing.

### A proposal for the future

I'd be lying if we didn't acknowledge the times that these models have landed a particularly complicated refactor. Or the magical moment when an AI agent anticipated what needed to be done and did it directly in the background. It's clear that the advances in technology are not going to stop. The annoyances I've listed here might not last another year at the rate of progress we're seeing in the industry.

On the other hand, let's not offload our thinking entirely, in order to stay knowledgeable and reputable. How might we...

* Think of less cumbersome ways to interact with LLMs than textually?
* Challenge ourselves to work unassisted for some time to better understand the system?
* Consider the use of these models in an assistive mode, rather than as a be-all owner?
* Keep the human in the loop, but make it an enjoyable loop?

I think tools are going to be the way forward. More on that in a bit.

An aside: I found myself recently on a flight back home with time to spare. I had intended to work with AI assistance (Claude Code) but my flight had wifi issues and I was offline for much of it. The two hours I spent digging in unassisted in the codebase gave me more insight and intuition than two months of work did.
