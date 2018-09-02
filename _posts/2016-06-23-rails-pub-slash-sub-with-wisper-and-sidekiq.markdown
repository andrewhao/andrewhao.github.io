---
layout: post
title: "Evented Rails: Decoupling domains in Rails with Wisper pub/sub events"
date: 2016-06-23 11:44
comments: true
published: true
categories:
- Domain-Driven Design
- DDD
- pub/sub
- Wisper
---

One common pattern in Domain-Driven Design is the use of publish/subscribe messaging to communicate between domains. When [Domain Events](http://martinfowler.com/eaaDev/DomainEvent.html) are created from within a domain, other domains are able to subscribe to these events and take action within their own domains, respectively.

This is not a common pattern in Rails, particularly because of Ruby's lack of language support for functional programming paradigms that exist in other languages. However, with a nifty framework and the help of Sidekiq, we can get just a little bit closer.

### What is a Domain Event?

A domain event is a recorded property in the system that tracks an action that the system performs, and the factors/properties that lead to its creation.

In the following examples, we are going to use the [Wisper](https://github.com/krisleech/wisper) gem to implement domain events in our sample [Delorean](http://github.com/andrewhao/delorean) app.

Imagine that we are writing an endpoint that our users will hit, indicating that they want to hail a time-traveling cab. Now the logic to hail a cab is rather complicated and lives in an entirely different area of the codebase, perhaps even in another application. How should we call the other code and ensure that our code is cleanly decoupled?

With our Domain-Driven powers, we've been smart enough to segregate our code into different subdomains and bounded contexts, denoted by these two Ruby modules `Ridesharing` and `DriverRouting`.

## Example 1: In-process pub-sub event modeling, with a service object.

A simple way to use Wisper is to use it to implement your service objects with Wisper, calling the service from the controller.

```ruby
module Ridesharing
  class RidesController < ApplicationController
    def post
      # Hail a time-traveling Delorean:
      command = HailDelorean.new
      command.on('hailed') { |driver|
        render text: "Hailed you a cab: #{driver} is arriving!"
      }
      .on('could_not_hail') {
        render text: "Sorry, no dice."
      }
      command.hail!(current_user)
    end
  end
end
```

Note that the `HailDelorean` class has powers of event subscriptions now. Our calling code does not have to concern itself with the implementation details of the `HailDelorean` service - it merely needs to register handlers for the two possible outcomes, `hailed` and `could_not_hail`. Here's how the service class is implemented:

```ruby
module Ridesharing
  class HailDelorean
    include Wisper::Publisher

    def hail!(user)
      # broadcast() is a Wisper method to fire an event
      driver = find_driver(user)
      if driver
        broadcast('hailed', driver)
      else
        broadcast('could_not_hail')
      end

    def find_driver(user)
      # Here lies slow, complex domain logic
      DriverRouting::FindDriver.new(user)
    end
  end
end
```

### Handling side effects in subscriber classes

Other side-effects can subscribe to the `HailDelorean` events. Let's say we want to fire an event to Segment analytics tracking. I can create a plain Ruby object that simply needs to implement a method with the same name as the event.

Let's implement `hailed` and `could_not_hail` methods on this subscriber class:

```ruby
class TrackSegmentAnalytics
  def self.hailed(driver)
    # fire analytics event to Segment
  end

  def self.could_not_hail
    # fire analytics event to Segment
  end
end
```

And we hook it up by subscribing it to the command handler:

```ruby
module Ridesharing
  class RidesController < ApplicationController
    def post
      # snip
      command = HailDelorean.new(current_user)

      # register the subscriber to the triggering action
      command.subscribe(TrackSegmentAnalytics)
      # snip
    end
  end
end
```

OK, that was a little awkward, doing all that wiring up in the controller. What if we did the wiring globally, within an app initializer?

```ruby
# config/initializers/domain_event_subscriptions.rb
Wisper.subscribe(TrackSegmentAnalytics, scope: "HailDelorean")

# alternate form:
HailDelorean.subscribe(TrackSegmentAnalytics)
```

This registers a global subscriber for all future instances of `HailDelorean`.

## Example 2: Asynchronous events with subscription handlers and Sidekiq

Here's the real power of Wisper - we can decouple our application domain responsibilities by modeling effects as subscription objects and do them out-of-band of the primary web request thread.

Note that with the [`wisper-sidekiq`](https://github.com/krisleech/wisper-sidekiq) gem, all subscriptions given with an `async: true` option flag will automatically execute in an external thread as a Sidekiq job. Let's take advantage of that now.

```ruby
module Ridesharing
  class RidesController < ApplicationController
    def post
      # Hail a time-traveling Delorean:
      HailDelorean.hail(current_user.id)
      render text: 'Hailing a cab, please wait for a response...'
    end
  end

  class HailDelorean
    include Wisper::Broadcaster

    def self.hail(passenger_id)
      broadcast(:hail, passenger_id)
    end
  end
end

module DriverRouting
  # Note that this class is both a subscriber and a publisher
  class FindDriver
    include Wisper::Publisher

    def self.hail(passenger_id)
      # Do slow, complex hairy routefinding/optimization/messaging behind the scenes:
      driver = find_driver_for(passenger_id)

      if driver
        broadcast('driver_found', passenger_id, driver.id)
      else
        broadcast('driver_not_found', passenger_id)
      end
    end
  end
end
```

Finally, we add handlers (subscribers) to these domain objects:

```ruby
module Ridesharing
  class NotifyPassengerWithDriverStatus
    def self.driver_found
      # send them a text message :)
    end

    def self.driver_not_found
      # send them a text message :(
    end
  end
end
```

Now let's link it together with subscriptions:

```ruby
# config/initializers/domain_event_subscriptions.rb
Ridesharing::HailDelorean.subscribe(DriverRouting::FindDriver, async: true)
DriverRouting::FindDriver.subscribe(Ridesharing::NotifyPassengerWithDriverStatus, async: true)
Wisper.subscribe(AnalyticsListener, scope: "Ridesharing::NotifyPassengerWithDriverStatus", "DriverRouting::FindDriver"], async: true)
```

Now our messages between our domains are pulled out of the main request thread, and operate in an asynchronous fashion with Sidekiq as the runner.

Code in our domains are kept clean - note that there are no direct references to the other subdomains within each subdomain. Our app more cleanly segregates the responsibilities between each app, heavy workloads are naturally balanced as they move to worker threads.

## Caveats: Beware of overbuilding

If you are on a small app, you probably should go with approach #1. The weight of indirection can be a cognitive load on development, unless you truly need to build async code in #2. The overhead and conceptual complexities of the approach can only be justified with large codebases, or in apps where a domain-centric view (and segregation) of code is present.

## Caveats: Event subscriptions can be a tangled mess

Note that the act of wiring can quickly fan out into a spidery mess of handlers - you could even further decouple your handlers by modeling a global event bus as a publisher, and having each domain tap into the bus' events and figure out how to handle each event on its own.

## Caveats: transactional consistency!

If you implement this asynchronously, you'll have to think about how to deal with transactional consistency. Can you design your data models (and database schema) to support independent updates without any dependencies? How will you handle the case when one domain action fails and the other completes?

You may have to roll your own two-phase commit here, the specifics of which I won't delve into. However, for most of our applications, we may want to skip the asynchronous and keep our events synchronous.
