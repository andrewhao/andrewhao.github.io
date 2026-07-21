---
layout: post
title: AI Nutrition Facts
categories:
- Coding
- Programming
- AI Slop
---

<h2 class="intro">Below is an edited copy of a memo I circulated at work. Various folks had been growing weary of reviewing AI slop. In this memo, I made the case for AI disclosures in written or generated artifacts. What do you think?</h2>

**tldr:** Because AI generated ~~slop~~ artifacts are everywhere now, I encourage you to disclose how you utilized AI in your work – “AI Nutrition Facts”. This helps your colleagues understand how to review your work, all while helping you think more about where gaps in your judgement might lie[^1].

## AI artifacts are everywhere

Let’s embrace the fact that AI augmented work is here to stay. Let’s also acknowledge that much of our AI artifacts are, with the current state of the art, prone to hallucination and mistakes. As much as I want to shake my fist and lament the existence of AI slop in work artifacts, I think it’s all good-faith work done by colleagues doing their best with the tools and models they have. Heck, I look at my own output and sometimes am surprised at how much of it is subtly wrong, overtly wrong, or just plain cringeworthy[^2].

Let’s also appreciate the fact that our colleagues on the other end – the ones reading and reviewing our code, memos and slide decks – are the ones who have the cognitive burden of reviewing this work. In this new AI generated content ecosystem, the verification and review of code is even more important and even more draining because as they read it, they need to know if the stuff they’re reading is unreviewed (or less-reviewed) slop or if it is fully represented by you.

> ## Disclosures can help!
>
> So here’s a simple suggestion: **Add a footnote (or tag) to your docs or PRs disclosing how you used AI:**
>
> * “AI was used to proofread and suggest grammatical edits”
> * “Claude/Gemini/ChatGPT was used to research and render this call sequence diagram”
> * “I read coworker’s design doc and had Claude create this slide deck from it”
> * `AI_NUTRITION_FACTS=No AI was used in the creation of this PR`
> * “Claude/Gemini/Codex was used to brainstorm and add to the ideas proposed here”

With disclosure, this helps your coworkers contextualize your work and better understand which parts of the doc they can trust from you - and which ones they may need to scrutinize more. By providing them more information, you reduce their cognitive load when reading your work.

Secondly, this benefits you. By actually thinking (and writing out) how you came to your answer, code, or artifacts, you are able to better understand what parts of your work need to be reviewed before you throw it over to your colleagues.

You might be thinking “Geez Andrew this is going to make me look bad if I let on how much of my work is AI generated!” I think the opposite is true - disclosing AI usage is a **gift of clarity** you give your reviewers.

So drop a disclosure into your work. Your coworkers will thank you for it.

[^1]: **AI Nutrition Facts:** Ideas are mine and I wrote this doc start to finish. AI was used to proofread and check narrative structure!
[^2]: Sorry team.
