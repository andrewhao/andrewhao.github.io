---
layout: post
title: Integrating Personas in User-Centered ML Model Development
date: 2019-12-07 21:13 -0800
categories:
- Machine Learning
- User-Centered Design
- Personas
---

<h2 class="intro">
For all the toil that went into developing the pipeline and deep learning model for my <a href="{% post_url 2019-10-06-what-training-an-audio-recognition-cnn-taught-me-about-parenting-and-about-humane-ml %}">ML baby monitor project</a>, I discovered that my model didn't actually solve the problem at hand. This kind of gap exists in the real world of ML development as well, and there's a critical insight from the field of User-Centered Design that could be the key to bridging it.
</h2>

As I’ve been growing my data science and machine learning chops, I’ve thought long and hard about what it means to develop a user-centered model. It seems to me that a lion’s share of ML energies are spent cultivating the data set and performing tricks to get the model architecture right.

After all, ML model development is hard - in addition to developing a data pipeline to arrive at a large data set that’s properly formatted, preprocessed, scaled, and trained on, there’s problems of balance, bias, out-of-training performance, and overall model performance.

## The deeper, more fundamental questions we should be asking

Developing the right model architecture and data pipeline, though flashy and news-article-worthy, is only one part of the process. And yes, amassing a large data set is fundamentally important in building an effective ML model, but yet again it misses some more fundamental challenges in developing ML products:

- Does this model actually solving a user need, or are we just “throwing machine learning at the problem” in hopes that something magical and powerful will appear?
- Is this model correctly modeling inputs to a user problem? Or are there other inputs into our system that we haven’t considered?
- If we implemented the machine learning system, are there second-order effects that would emerge that we haven't considered?

In my [last article]({% post_url 2019-10-06-what-training-an-audio-recognition-cnn-taught-me-about-parenting-and-about-humane-ml %}) and in my [PyGotham talk](https://www.youtube.com/watch?v=sT_yS8XAQEw), I argued that the best way to ground the model in the problem domain is to radically keep the user front-and-center in the entire development lifecycle. As it turns out, the field of User-Centered Design has been advocating for this thinking for the past couple of decades.

## What is User-centered Design? What are personas?

Central to the discipline of **user-centered research** are *Personas*, which are fictitious representations of real users that inform our development work. Developed by [programmer Alan Cooper in the 80's](https://www.cooper.com/journal/2008/05/the_origin_of_personas/), Personas:

- Are fictional characters that the team creates, heavily based on real customers
- With names, hobbies & life tidbits to bring them to life
- Narrating their objectives, fears, and goals relating to the product

When the whole team internalizes the personas, it helps focus their work. They can now refer to users by name, considering whether each product feature, or machine learning capability they develop will impact their customer personally.

Personas, in short, animate our customer and put them first in every product- (and data-) decision that we make.

## Meet the team

Imagine a scene playing out in the engineering team at AcmeWidgets.com, where our team is developing a recommendation system for the e-commerce retailer to surface relevant items on product pages. To do this, a crack team of ML engineers and data scientists are gathered to build the system. 

The team begins by defining an *objective function* for their model. In this case, the team decides to choose a function that maximizes shopping cart value (our objective function). For example, it may discover that users have a propensity to splurge on a set of high-ticket-value items in our catalog.

However, things soon go awry. The team notices that their model ends up making low-quality recommendations that sacrifice long-term customer retention for short-term boosts in average order value. The team discovers that their model ends up pushing customers to purchase items that they don’t really want or need, decreasing the long-term loyalty of the customer to the product and the business. In fact, the products that the recommendation engine suggests end up being returned at a far higher rate, incurring operating expenses and costs for the business.

Well surely that's just a matter of tweaking the model! The team goes back to the drawing board and this time launches a model that changes the objective function of the model to minimize the return rate of its products. The model is tested and voila, the system is again chugging along happily again. Users seem to be happy with their purchases...

...until three months down the line, when they discover that users that interact with this recommendation engine are actually churning and leaving the product at far higher rates than those who do not see the recommendations! As it turns out, these recommendations have upset and angered users so much that they do not even bother returning.

And so the team goes back to the drawing board, dejected and feeling a sense of foreboding of what else they might find in the next iteration of their project.

## What went wrong?

If we went back and sat down with the team at their project retrospective, we might have heard reflections like:

- *We didn't consider the full suite of user lifecycle metrics in our model*
- *We didn't think holistically enough*
- *We rushed to market believing we understood the right way forward*
- *There were customer edge cases we didn't consider*

Of course, many of these things can only be learned from experience! But could there have been a better way forward for the project?

The key issue is that team was only thinking at the *tactical* level. They thought - "oh, a recommendation engine should be simple. We'll just pull in Architecture X for our model, train against objective Y and ship it when we can attain model performance Z".

## Personas get us into the customer's shoes, so we can develop expert intuition

ML practitioners will often tell you that a great ML system blends *domain expertise* and *expert intuition*. What this means is that ML models must be designed with team members who have deep understanding with the business domain, a deep familiarity of the data set, and a deep intuition of the customer's needs.

How do you get domain expertise? Well, you have to have the right people embedded on the team, solving the right problems, considering the customer at all points in the model development process. How might having user personas have helped us avert this situation?

Imagine that the team, back in the beginning, agreed to consider their customers as living, breathing people in the real world. In fact, their UX colleague did a set of customer interviews that resulted in a set of composite sketches of their customers:

*Luke, the office admin*

| What | Description |
| ---- | ----------- |
| Profile | Luke is a 31-year old male living in Indianapolis and works as an office admin for a small fulfillment business |
| Motivations | Luke needs to keep widgets stocked in the office for the employees to use. Given that the office runs out of widgets regularly, he needs to keep them stocked every couple of weeks. He dreads having to log back in to the web site to place another order since he finds it tedious. |
| Goals | Seamless, regular order re-fulfillment for the same SKU |

*Harmony, the wedding planner*

| What | Description |
| ---- | ----------- |
| Profile | Harmony is a 41-year old female who lives in Boston and runs an event-planning business that is just getting off the ground. She loves to supply Acme Widgets at her events because they are loved by the guests and provide her business the visibility she needs. |
| Motivations | Harmony has dreams to build an event-planning empire in her city |
| Goals | Unique, hard-to-find, desirable widgets to make her business stand out |


*Tianqi, the Work-From-Home dad*

| What | Description |
| ---- | ----------- |
| Profile | Tianqi is a remote freelancer who lives in Shanghai, who enjoys his work-from-home flexibility because it allows him to do childcare for his family while his partner works full-time. |
| Motivations | Tianqi wants an ordered, efficient home life on a shoestring budget |
| Goals | Buy what I want at a budget |

## How to integrate personas into model development

The reason we might want to consider personas is so we can firsthand understand our customers' *goals* and *motivations*. Harmony wants to discover unique items that pop. Luke just wants to fulfill a recurring order for the office. Tianqi wants the cheapest items, period.

The team, when considering their recommendation algorithm, should be actively referring to Harmony, Luke and Tianqi by name as they develop their model --

> TEAMMATE 1: If we choose to use the collaborative filtering model, we have to consider the fact that we have a very small sample size of users who match up to Luke's use case (power fulfiller). I'm worried that Luke's cohort is just going to see a lot of garbage recommendations before any meaningful signal emerges, and get turned off by our product.
>
> TEAMMATE 2: Yeah, that's true. But we know that Harmony's cohort accounts for over 75% of our sales. There's lots of opportunity here. What if we went with a bandit approach? That should hopefully minimize the number of bad recommendations that get out in front of our users.
>
> TEAMMATE 1: Not a bad idea. We could learn pretty quickly given the scale that we operate at, and we'd minimize the amount of time we serve bad recommendations to Luke's cohort.

Or consider another scenario in the development process, where the team has to grapple with negative interactions with the recommender in the wild:

> TEAMMATE 1: We've received some customer feedback on Twitter that our products are too expensive. Apparently some customers feel like they were duped into purchasing items they didn't need - the Tianqi cohort. How can we be sure that folks are truly receiving valuable and worthy recommendations?
>
> TEAMMATE 2: Why don't they feel like their items are valuable?
>
> TEAMMATE 1: For some reason, our recommendation system is creating some sort of buyers' regret. We might be over-hyping some of our promotions, or we may be pushing some items that have had quality issues.
>
> TEAMMATE 2: Let me start a conversation with Juli, our UX designer, to see if there's some sort of user research that would confirm or validate this hypothesis. And if there's some sort of feature we need to re-incorporate into our model to make sure we're making truly high-quality recommendations, let's incorporate it. Else if our recommendations aren't up to snuff, let's test a new model variant where we don't show any results at all.
>
> TEAMMATE 2: Got it.

See? That's the kind of holistic, customer-centric conversation we want to build into the team's natural modus operandi. And all those things are enabled by customer personas.

## In conclusion

Personas are powerful - and they're not a silver bullet. What personas help us do is imagine our customers as real people, with real goals, motivations and frustrations. Doing so will give us the vocabulary to discuss them as first-class citizens to then orient our technical solutions around.

Oh - and by the way, the People & AI Research (PAIR) team at Google has done far more thinking about the process of designing human-centered AI products. [Have a read through their best practices guide](https://pair.withgoogle.com/) to learn from their deep experience building ML products.

*What do you think? Do you have experience building ML models in customer-centric ways? Reach out and let me know on Twitter at [@andrewhao](https://www.twitter.com/andrewhao)*.

## References

- [Google - People + AI Research](https://design.google/library/ai/) - Google Design
- [A Closer Look at Personas](https://www.smashingmagazine.com/2014/08/a-closer-look-at-personas-part-1/) - Smashing Magazine
- [The Origin of Personas](https://www.cooper.com/journal/2008/05/the_origin_of_personas/) - Alan Cooper
