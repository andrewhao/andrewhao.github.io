---
layout: post
title: What linguistics can teach us about building software with distributed teams
date: 2021-07-13 22:21 -0700
comments: true
categories:
- Linguistics
- Software Design
- Semiotics
- Architecture
---

<h2 class="intro">What can semiotics reveal about the hidden cultural frictions in distributed software development?</h2>

_This post originally appeared as a guest article on [LeadDev.com](https://leaddev.com/managing-distributed-teams/what-linguistics-can-teach-us-about-building-software-distributed-teams)._


## Worst day ever

It hasn’t been a good Monday afternoon. A remote team checks something into the Authentication service codebase that breaks the User Profile service owned by your team. By the time the deploy is rolled back, customers haven’t been able to log in to your product for several hours and news outlets have picked up the story. The CEO is on the line, demanding answers. You send hasty apology emails to your customers, then sign off for the day, exhausted. 

In the days to come, everybody has an opinion about what could have gone better. Some  suggest the teams needed better external documentation. Others suggest that a signoff process should be enforced and project management should get more involved. Another director suggests starting yet another architecture review committee. Although all those sound like good ideas, something nags at you - what’s _actually_ happening here?


## Software as written, cultural artifact

When we think of a software system, we often conceptualize it in its dynamic, operationalized form in production. We ask questions like, _how many transactions per second can it process under load? Is it meeting SLO targets? _But we also need to remember that software systems take a form much like written communication between the developers working on the system. It is just as important for a software system to have several nines of uptime as it is for it to be readable and understandable to the engineer poring over the code, trying to make sense of its shape.

After all, we know that engineers spend more time _reading_ code than writing it. If an engineer makes a change to a system with an incorrect mental model, then defects will emerge._ _And the team’s mental model is indelibly imprinted in the code, the tests, and the documentation.

Software’s purpose is not just to achieve business goals for the company, but to be easily changeable, elegantly designed, and robustly tested for _all current and future_ members of the team. In other words - one of the primary purposes of software is to guide its readers into constructing a mental model of the world and how it works.

What if we tried to imagine our systems as if they were time capsules - artifacts left behind for future teams and collaborators to sift through and understand? I believe that software systems can be thought of as _textual artifacts_, messages in a bottle, if you will, to a future teammate or cross-org collaborator, meant to convey the shape and meaning of the system in this current point in time.

To do that, I want to take you on a detour through _semiotics_, a field of linguistics and communication theory that deconstructs meaning in everything from literature, TV ads, political messaging, and Internet memes. What could that have to do with software development?


## Intro to semiotics

Emerging from the work of Ferdinand de Saussure in the early 1900s, [semiotics](https://en.wikipedia.org/wiki/Semiotics) is the study of signs and symbols as they make meaning in cultural communications. Saussure was a linguist interested in how meaning was constructed through language. In Saussure’s model, meaning is constructed by a **Signifier**, a concrete “thing” in the world, and its corresponding **Signified** concept (a connoted meaning). Take the example of this image:

![Image of a rose (flower)](/images/linguistics-and-software-development/rose_small.jpg "Image of a rose (flower)")

_Photo by [Carlos Quintero](https://unsplash.com/@c_quintero?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/rose?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)_

The **Signifier** is the representation of this rose on your screen, pixel by pixel. By itself, it doesn’t communicate any meaning. Now you, the viewer, see this image and may think to yourself - _ah, a rose!_ and automatically think about the _flower_ (the **Signified** concept). How did you know that? You have familiarity with this type of flower in your lived experience, having seen roses in flower shops and in gardens around you.

But if you saw this image of a rose on a highway billboard for a jewelry store or in an online Valentine’s Day floral service ad, you may see different layers of meaning. You may understand this rose as signifying the concept of _Romance, Love, or Passion_, based on your cultural understanding of roses and the role they play in cultural tropes in movies, film, TV.

But just one second - if you were from a non-Western background (or an intergalactic alien being), this image of a rose may mean nothing to you!


<table>
  <tr>
   <td><strong>Signifier (Concrete)</strong>
   </td>
   <td><strong>Context</strong>
   </td>
   <td><strong>Signified (Concept)</strong>
   </td>
  </tr>
  <tr>
   <td>An image of a rose
   </td>
   <td>On a billboard for a jewelry ad    </td>
   <td>The concept of “Romance”
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>In a gardening textbook
   </td>
   <td>The concept of the flower known as the rose
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>A non-Western context
   </td>
   <td>?
   </td>
  </tr>
</table>


Saussure developed the beginnings of a framework for how meaning is communicated and understood through concrete artifacts in the world. This framework of semiotic analysis allows us to deconstruct a message into constituent parts - the actual form of the idea in a concrete form, its metaphorical or connoted meaning, and the role of the receiver as the message is parsed in context.

So what does this all have to do with software development? Semiotics pinpoints the hidden role of cultural assumptions of different viewers in different contexts looking at the same things!  Let’s get practical and try to apply some semiotic thinking to highlight how divergent understandings can arise from seemingly innocuous features of our systems.


## Naming

The first and most obvious place to apply semiotic thinking is in the _naming_ of the concepts made concrete in our software. Go through your system and make a list of the concepts encoded in class names, variable names, functions and even database tables. You may observe:



* Names may be internally consistent, from years of legacy system drift
* Names may reflect unique business processes or concepts that are unfamiliar to an external reader
* Names for concepts (like a _User_ or an _Account_) may have a specific meaning within your business unit, but may have slightly different meanings to teams in other business contexts.

For example…

* A _User_ to your team may refer to a system record that contains login and authentication fields (like password authentication).
* However, a _User_ to an external team that builds the core product flow may associate it with user metadata, like a public profile.
* Though your system may, in fact, use the same database table to store the two, the two concepts are different enough to draw separate distinctions.
* This may lead to mistaken assumptions about key features about your _User Profile API_, which may be assumed to model one part of the system when in reality it does not.

<table>
  <tr>
   <td>
<strong>Signifier (Concrete)</strong>
   </td>
   <td><strong>Context</strong>
   </td>
   <td><strong>Signified (Concept)</strong>
   </td>
  </tr>
  <tr>
   <td>The <em>User </em>in the database
   </td>
   <td>The Identity team   </td>
   <td>A record that maintains core account authentication attributes
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>The Core Product team
   </td>
   <td>A record that maps to the user’s unique presence in the world as they use the core product - for example, a place to store Profile information
   </td>
  </tr>
</table>


Aha! That’s a key insight - that the perspective of a teammate in the Identity organization leads them to understand the User model slightly differently from a teammate in the Core Product organization.


### Make it clear

To tackle this challenge and make these cultural assumptions explicit, draw from disciplines like [Domain-Driven Design](https://en.wikipedia.org/wiki/Domain-driven_design) where the nuances in language in business contexts are made explicit in code. You may consider:



* Creating a [Ubiquitous Language](https://martinfowler.com/bliki/UbiquitousLanguage.html) of terms and definitions, written down in a _Glossary_ to capture distinctions about the concepts in your corner of the world. This comes in handy for external readers who may be unfamiliar with the nuances of your world.
* Running a _[Context Mapping](https://www.infoq.com/articles/ddd-contextmapping/)_ or _[Event Storming](https://www.eventstorming.com/)_ exercise to collectively create a working mental model of your system, and bring out the names of concepts that run your business. This is particularly useful if there has not previously been much thought given to naming before.
* If you depend on a “black box” system (an ML model, an API) upstream where the definition of the data is unclear, this would encourage your team to contact that team to build that shared understanding together.
* If it becomes clear that your system is internally inconsistent or does not match the language of the business, then it would behoove your group to undergo some maintenance to rename your concepts in code for clarity.


## Structure

The structure, or form, of our software systems is another concrete feature that communicates meaning. Consider these considerations around the architecture, platform and runtime features of our system:



* Your team may have chosen a dynamic, convention-over-configuration language and framework like Ruby on Rails because you valued shipping features quickly while you found product-market fit. Or the team may have chosen Go because it valued performance, or you may have chosen Python because of its natural integration into the data science workflow.
* Your chosen data store is DynamoDB because you have to handle huge, spiky surges of  production traffic. Alternatively, you chose a relational database over a key-value store because you value normalized data and consistency for the query patterns you have to sustain.
* Your usage of Event Sourcing architecture is due to an architect’s new vision for a highly scalable architecture with first-class auditability.

Many of these choices are not obvious to our new teammates or collaborators, who may have their own cultural or organizational constraints. “Why is feature X built with Y?” they may ask. If you don’t answer these questions up front, they may project their own assumptions incorrectly on your systems.

For example:



* Your team has chosen an Event Sourcing architecture, which eschews typical CRUD database operations for immutable events on a message bus. 
* However, your collaborator teammates on the other side of the company are unfamiliar with the paradigm since that architecture is new to their company unit.
* This mismatch in mental models leads their team to call an undocumented API endpoint that they believed was safe for use, but in reality emitted an event that corrupted the state of the system
* Leading to the site outage that fateful Monday…

<table>
  <tr>
   <td>
<strong>Signifier (Concrete)</strong>
   </td>
   <td><strong>Context</strong>
   </td>
   <td><strong>Signified (Concept)</strong>
   </td>
  </tr>
  <tr>
   <td>The (undocumented) <em>User Profile API</em>
   </td>
   <td>The Identity team</td>
   <td>A protected API endpoint that is meant only for use in manual data migrations. If misused, it will emit invalid events that could corrupt the event store.
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>The Core Product team
   </td>
   <td>An API endpoint that allows our service to store profile data.
   </td>
  </tr>
</table>



### Make it clear

To solve for this, explicitly write all architectural and structural decisions down:



* Creating an **Architecture master document** detailing the system’s shape, key features, and primary concerns can be a great go-to reference for new teammates and collaborators. Here you may link important technical specifications or design documents that were instrumental to creating the system.
* **[Architecture Decision Records](https://adr.github.io/)** capture important decisions at different points in time that give readers context to why decisions were made the way they were made.
* **System diagrams (or anything visual)** communicate the shape of the system far quicker than any word-filled paragraph can.
* Documenting **Value Trade-offs** can be helpful ways for other teams to understand what your system is meant to optimize toward. For example:
    * “We chose to build on Ruby on Rails because we value shipping features quickly over being overly obsessed about performance.”
    * “We have an event-sourced architecture, so we prefer to think in terms of events over records of truth. This also makes auditability a first-class feature of our systems, which is important in the financial payments space we are in“
    * “We use random forest models over neural networks because we value model interpretability over overall accuracy, especially since we our business model carries legal exposure”


## Writing software is intercultural communication

Through our crash course in semiotics, we’ve learned how it can identify structures in our software systems that are open to (mis)interpretation. By getting ahead of ourselves and thinking about how different teams in different parts of the organization parse and interpret our systems and flows, we can anticipate ways in which we can end up with divergent mental models. We can get ahead of the issues to develop habits to document the hidden cultural forces that shape our code, our architectural decisions, and our software use cases.

Your software system is a message in a bottle. When a stranger in the future picks it up on some faraway sandy shore, what stories will it tell?
