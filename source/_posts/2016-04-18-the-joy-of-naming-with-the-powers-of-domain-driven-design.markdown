---
layout: post
title: "Domain-Driven Design & The Joy of Naming"
date: 2016-04-18 17:14
comments: true
categories:
- Domain-Driven Design
- Architecture
- DDD
---

I want to discuss a topic near and dear to my heart, and what I believe
is at the crux of effective software design. It's not a new functional language,
it's not a fancy new framework, a how-to guide to do microservices, nor a quantum leap in the field of machine learning.

It's much simpler.

It's about names.

![In the beginning...](http://3.bp.blogspot.com/_rL73HKlqbH0/TKhVHMbs2oI/AAAAAAAABEc/DiPbblM0R44/s1600/sistine-chapel-michelangelo-paintings-6.jpg)

*Names define us*. They define concepts. They imbue a concept with shared understanding. They're language concepts, but more than that, they're units of meaning.

Software development is a fundamentally human endeavour. No amount of technical computing breakthroughs will change the fact that software development is still the arduous task of getting a team together full of humans from a kaleidescope of different cultural, linguistic backgrounds - then throwing them together to build an arbitrarily complex product in a rapidly-shifting competitive landscape.

Not only that, the thing to build is chock-full of systems that interact with other systems of unbounded complexity. Additionally, once your software system is out in the wild, you need to make sure that it was the right thing to build. Is the product you built correctly tuned to your market? Is it generating sufficient revenue?

The landscape is littered with software projects that began ambitiously, but got lost in a towering mess of fragile code. It's no wonder that developing reliable, successful software is more art than science.

### Crossing our linguistic wires

Let's rewind back to a scene from a typical day in the life of your software development team. Think back to the last time you discussed a story with your product owner, how did it unfold?

Let's imagine a scene at Delorean, the Uber for time travel, where you work:

> PO: Our next big project is to update our driver app to show rider locations on the timeline map.

> You: And when do these riders show up on the timeline map?

> PO: When the driver turns on the app and signals that she's driving.

> You: OK, so that means when the app boots up and the DriverStatus service receives a POST we'll need to simultaneously fetch references from the HailingUser service based on time locality.

> PO: Um... I guess so?

Or how about your last iteration planning meeting, where you discussed the intricacies of a specific story?

> PO: In this story, we're going to add a coupon box to the checkout flow.

> You: [Thinking out loud] Hm... would that mean we add a `/coupon` route to the checkout API?

> Teammate: Wait - I think we call them `Discounts` in the backend. And the checkout flow is technically part of the `RideCommerce` service.

> You: Right - I mean let's call the route `/coupon` but it'll create a `Discount` object. And in this story,    let's just remember that the checkout API really refers to the `RideCommerce` service.

> PO: I'll add a note to the story.

The implementing engineer, of course, doesn't read the note in the story (who has time to, anyways?). In the course of implementation, he gets tripped up in semantics and spends the better part of a half day re-implementing the `Checkout` flow as an entirely new service, before realizing his mistake in code review and backing out his changes.

Months later, a new colleague is tasked to fix the link in the checkout flow, but files an incomplete fix because she was not aware of the fact that `Coupons` actually had mappings back to `Discounts`. The bug makes its way to production, where it subtly lies dormant until a most inopportune time...

### A better, Domain-Driven way

In Eric Evans' book [__Domain-Driven Design__](http://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215), he describes the concept of a [Ubiquitous Language](http://martinfowler.com/bliki/UbiquitousLanguage.html) - a shared, common vocabulary that the entire team shares when discussing software.

When we say the "entire team", we mean the combined team of designers, developers, the product owner and any other domain experts that might be at hand.

Your product owner may be your domain expert (and typically is). However, you may have other domain experts such as:

* Any team that builds reporting or analytics off of your software.
* Upstream data providers
* Anybody further up the reporting chain whose purview includes the software you're building, or its effects. Think: the Director of Finance, the COO, the head of Customer Support.
* The users of your software

Side note: in XP, each team has an "onsite customer" - this is your domain expert!

#### Developing a Ubiquitous Language with a Glossary

Try this: keep a living document of all the terminology your team uses - along with all its definitions. This __Glossary__ is exactly what it sounds - a list of terms and their definitions.

> Delorean Team Glossary
> ----------------------
>
> - __Coupon__: an applied discount to a BookingAmount. A coupon may take the form of a Fixed or a Percentage amount.
>    - __Fixed-type__: A coupon that applies a fixed amount of money - e.g. a $30 USD discount.
>    - __Percentage-type__: A coupon that applies a percentage savings off the total BookingAmount.
> - __Driver__: An employed driver who drives within the system, picking up passengers and driving Trips for payment.
> - __Trip__: An itinerary of passenger pick-up and drop-off location and times.
> - __Rider__: The passenger that books the trip and is transported by the *Driver*.
> - __Booking__: A reservation for a Trip, as booked by the *Rider*.
> - __BookingAmount__: The monetary amount of the Trip, accounting for the trip cost, surge pricing, coupons and taxes.
> - __Routing Engine:__ The software system that maps out the driving directions for a driver.
> - __Payment__: A record of how a user paid.
> - __Charge__: A financial transaction for a specific dollar amount, for a specific charge method to an institution.
> - __Checkout__: A workflow in which a *Payment* is made for a *Booking*.

From now on, use only the term definitions listed here in your stories. Be explicit about how you use your language!

I've been on many projects where the sloppy usage of a term from project inception led to the usage of that term in the code - codifying that messy, slippery term throughout the life of the project.

Which leads us to our next point:

#### Refactoring your team to use the right terms

Your **Glossary** is a living document. It is meant to be living - either on a continually-updated Google Doc or a wiki page. It should be visible for all to see - you should print it out and post it on the walls!

Meanwhile, in a planning meeting:

> You: So when a user logs into the app and broadcasts that they're ready to drive...

> PO: You mean *Driver*. When a *Driver* logs in.

> You: Right. Good catch.

It seems a little silly (after all, you both know only Drivers use the broadcast feature of the app), but the laser focus on using the right words means that your team is always on the same page when talking about things.

Later that afternoon, your teammate taps you on the shoulder:

> Teammate: I'm about to implement the Coupon story. I suggest we rename the `Discount` class to `Coupon`.

> You: Great idea. That way, we aren't tripped up by the naming mismatches in the future.

> Teammate: I do have a question about the coupon, though. Do you think it's *applied* to the **BookingAmount**, or is it *added*?

> PO: [Overhearing conversation] You had it right. It's *applied*.

You and your teammate then go and update the glossary, scribbling an addendum on the wall (or updating your wiki):

```
- **Coupon**: ... Coupons may be *applied* to BookingAmounts to discount the total cost of the booking.
```

#### Refactoring your code to use the right terms

Your teammate and you then walk over to her desk; as a pair you proceed to refactor the existing account code. We'll use Ruby for the sake of this example.

In the beginning, the code looks like this:

```ruby
class Checkout
  def initialize(booking_amount, discount)
    @booking_amount = booking_amount
    @discount = discount
  end

  def total
    @booking_amount.total - @discount.calculate_amount_for(booking_amount: booking_amount)
  end
end

class Discount
  STRATEGY_FIXED = 'STRATEGY_FIXED'
  STRATEGY_PERCENTAGE = 'STRATEGY_PERCENTAGE'

  def initialize(amount, strategy)
    @amount = amount
    @strategy = strategy
  end

  def calculate_amount_for(booking_amount:)
    # Implementation...
  end
end
```

You take a first pass and rename the `Discount` class to `Coupon`.

```ruby
class Coupon
  STRATEGY_FIXED = 'STRATEGY_FIXED'
  STRATEGY_PERCENTAGE = 'STRATEGY_PERCENTAGE'

  def initialize(amount, strategy)
    @amount = amount
    @strategy = strategy
  end

  def calculate_amount_for(booking_amount:)
    # Implementation...
  end
end
```

Now there's something funny here - your domain language suggests that a **Coupon** is *applied to* a **BookingAmount**. You pause, because the code reads the opposite - "A Coupon calculates its amount for a BookingAmount".

> You: How about we also refactor the `calculate_amount_for` method to reflect the language a little better?

> Teammate: Yeah. It sounds like the action occurs the other way - the BookingAmount is responsible for applying a Coupon to itself.

In your next refactoring pass, you move the `calculate_amount_for` method into the `BookingAmount`, calling it `applied_discount_total`:

```ruby
class BookingAmount
  # implementation details...

  def applied_coupon_amount(coupon:)
    # Implementation...
  end
end
```

Finally, you change your `Checkout` implementation to match:

```ruby
class Checkout
  def initialize(booking_amount, coupon)
    @booking_amount = booking_amount
    @coupon = coupon
  end

  def total_amount
    @booking_amount.price - @booking_amount.applied_coupon_amount(coupon: @coupon)
  end
end
```

When you read the implementation in plain English, it reads:

> The checkout's total amount is calculated by subtracting the booking amount's applied coupon amount from the booking amount price.

Phew! Designing a strong **Ubiquitous Language** was hard work! In fact, you had spent a goodly amount of time debating and clarifying with your domain experts:

* Is a Coupon *applied* to a BookingAmount, or is it *discounted from* one?
* Should we call it a Coupon *amount*, or a Coupon *cost*?
* Is the pre-tax, pre-discount amount in the BookingAmount called a *price*, or a *cost*?

Whatever you agreed on, that's what you changed your code to reflect.

#### Continual refinement

Hm. Something still feels off.

You and your teammate feel your OOP spidey senses going haywire.

> Teammate: Hm. I guess that worked, but that's still not exactly as clean as we wanted it. Isn't it kind of weird how the Checkout owns the calculation for the calculation of a discount?

> You: Yeah, I see where you're coming from. That's just not good OO design. Additionally, if we notice the language our domain experts were using, they didn't mention that the checkout total was some subtraction of something from another thing. The Checkout's total simply is the order amount, after application of a Coupon.

Your partner and you take one last step:

```ruby
class Checkout
  def initialize(booking_amount, coupon)
    @booking_amount = booking_amount
    @booking_amount.apply!(coupon)
  end

  def total_amount
    @booking_amount.amount
  end
end


class BookingAmount
  # Implementation...

  def apply!(coupon)
    @coupons += coupon
  end
  
  def amount
    @amount - @coupons.sum(&:amount)
  end
end
```

You sit back and read it back, out loud:

> The checkout's total amount is the BookingAmount after a Coupon has been applied.

You both smile. Much better.

### In closing...

In this brief time we had together,

* We discussed why names are important - especially in a complex endeavour like software development.
* We covered why it's important to arrive at a shared understanding, together as a team, using the same words and vocabulary.
* We discovered how to build and integrate a **Glossary** into the daily rhythm of our team
* We refactored the code twice - illustrating how to get code in line with the domain language.

### And there is much more!

In an upcoming post, we'll investigate how the **Ubiquitous Language** applies to a core concept of Domain-Driven Design: the **Bounded Context**. Why is that important? Because Bounded Contexts give us tools to organize our code - and to do further advanced things like [break up monoliths into services](https://speakerdeck.com/andrewhao/ddd-rail-your-monorail). 

<script async class="speakerdeck-embed" data-id="1e6dd8983891467381036a321cd274a9" data-ratio="1.77777777777778" src="//speakerdeck.com/assets/embed.js"></script>
