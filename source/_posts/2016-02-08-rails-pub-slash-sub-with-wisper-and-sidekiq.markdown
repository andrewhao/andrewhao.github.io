---
layout: post
title: "Decoupling domains in Rails with Wisper pub/sub"
date: 2016-02-08 11:44
comments: true
published: false
categories: 
- Domain-Driven Design
- DDD
- pub/sub
- Wisper
---

One common pattern in Domain-Driven Design is the use of
publish/subscribe messaging to communicate between domains. When
[Domain Events]() are created from within a domain, other domains are
able to subscribe to these events and take action within their own
domains, respectively.

### Sidebar: What is a Domain Event?

A domain event is a recorded property in the system that tracks an
action that the system performs, and the factors/properties that lead to
its creation.

## Example 1: Local pub-sub

A simple way to use Wisper is to use it to implement your service
objects within the larger scope of your controller. For instance, your
controller objects are going to need

```ruby
module Ridesharing
  class RidesController < ApplicationController
    def new
      # Hail a time-traveling Delorean:
      command = HailDelorean.new.hail!
      command.on('hail
    end
  end

# The HailDelorean class is a service object that finds a car and
# gets it to come to you.
class HailDelorean
  include Wisper::Publisher

  def hail!
    # broadcast() is a Wisper method to fire an event
    if (perform_find_vehicle == 'success')
      broadcast('hailed', user: user)
    else
      broadcast('oops')
    end

  def find_vehicle
    # Something fancy.
  end
end
```

## Example 1: Global pub-sub

In the following example, we are going to use the [Wisper]() gem to
implement domain events in our sample [Delorean]() app.

```ruby
module Ridesharing
  class RidesController < ApplicationController
    def new
      # Hail a time-traveling Delorean:
      HailDelorean.new.hail!
    end
  end
end

# The HailDelorean class is a service object that 
class HailDelorean
  def hail
    DomainEventPublisher.new.publish('hail_delorean', user: user)
  end
end

class DomainEventPublisher
  include Wisper::Broadcaster
  def publish(event_name, params:)
    # Broadcast is a Wisper
    broadcast(event_name, params)
  end
end
```


## Caveats: transactional consistency!

If you implement this asynchronously, you'll have to think about how to
deal with transactional consistency. Can you design your data models
(and database schema) to support independent updates without any
dependencies? How will you handle
the case when one domain action fails and the other completes?

You may have to roll your own two-phase commit here, the specifics of
which I won't delve into. However, for most of our applications, we may
want to skip the asynchronous and keep our events synchronous.
