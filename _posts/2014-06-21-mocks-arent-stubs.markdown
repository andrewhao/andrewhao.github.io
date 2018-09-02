---
layout: post
title: "Mocks aren't stubs: mockist & classic testing"
date: 2014-06-21 17:55
comments: true
categories: 
- TDD
- mocks
- stubs
---
With the famed "TDD is dead" debate around the Rails community largely
coming to an end, I found myself referencing Martin Fowler's article,
[Mocks Aren't Stubs](http://martinfowler.com/articles/mocksArentStubs.html) a good deal, trying to make sense of it in terms of how I write tests and code.

In this post I'm going to talk about mocking and stubbing and their
roots, paraphrase Fowler in an attempt to explain their differences, and
walk through a couple of code examples. In each case, I'm going to
attempt to build this example out in Ruby and RSpec 3.

Let's assume this implementation in our code for a `BookUpdater` object in Ruby. Its job is to call through its collaborating `ApiClient`, which wraps some aspect of an API that we want to call.

```ruby
# Update a book's metadata in our systems.
class BookUpdater
  attr_accessor :book, :api_client, :response

  def initialize(book, api_client)
    @book = book
    @api_client = api_client
  end

  def updated?
    !!response.success?
  end

  def update!
    response = api_client.call_update_api(book)
  end
end
```

## What they are

### Mocks

Mocks are fake objects that verify that they have received messages. In
RSpec, we traditionally use the `mock` object utility to create these objects.

```ruby
api_client = mock('api client')
book = Book.new

expect(api_client).to receive(:call_update_api).with(book).and_return(true)

subject = BookUpdater.new(api_client, book)
subject.list!
```

What's happening here? RSpec creates a mock `api_client` object that will verify that, after the test case executes, it has received the `:call_update_api` message with the correct arguments.

The main point of this style of testing is *behavior verification* -- that is, that your object is behaving correctly in relation with its collaborators.

### Double

Let's take a look at a `double` -- also known as a `stub`. A `double` is a fake object that is set up to respond to a certain message with a pre-canned response, each time. Let's take a look at how I would set up a test using doubles:

```ruby
api_client = double('api client')
response = double('response', :success? => true)
book = Book.new

allow(api_client).to receive(:call_update_api).with(book).and_return(response)
expect(subject.update!).to change(subject, :updated?).from(false).to(true)
```

Okay, so what's the big deal here? My test case still passes. Note that
I had to change my code to focus its expectation on the `subject`'s
state instead of the `api_client`.

The focus of using doubles is for *state verification* -- that is, that so long as everybody around me is behaving according to their contracts, the test merely has to verify that internal object state changes correctly.

### A third way -- real objects

I won't cover this very much in depth, but with sufficiently simple objects, one could actually instantiate real objects instead of doubles, and test an object against all its collaborators. This is, in my experience, the most common experience of working in Rails + ActiveRecord applications.

## Classic vs Mockist testing: different strokes for different folks

As we saw above, the key difference between the mock and the stub (the `double`). The focus of the test in the mock case is on the messages being sent to the collaborators. The focus of the test when using the double is on the the `subject` under test (SUT).

Mocks and stubs/doubles are tools that we can use under the larger umbrellas of two TDD philosophical styles: *classic* vs *mockist* styles.

### Classic TDD

* Classic TDDists like using `double`s or real objects to test collaborators.
* From personal experience, testing classicly is oftentimes the path of least resistance. There isn't expectation setup and verification that mockist testing requires of you.
* Classic TDD sometimes results in creating objects that reveal state -- note how the `BookUpdater` needed to expose an `updated?` method.
* Setting up the state of the world prior to your test may be complex, requiring setting up all the objects in your universe. This can be a huge pain (has anybody ever had this problem with overcomplicated Rails models with spidery associations? Raise your hands...). Classicists may argue that the root cause here is not paying attention to your model architecture, and having too many associations is an antipattern. Alternatively, classicists oftentimes generate test factories (e.g. Rails' FactoryGirl gem) to manage test setup.
* Tests tend to be treatable more like black boxes, testable under isolation (due to verifications on object state) and are more resistant to refactoring.

### Mockist TDD

* Mockist TDD utilizes `mock`s to verify behavior between objects and collaborators.
* It can be argued to develop "purer" objects, that are mainly concerned with objects passing messages to each other. Fowler refers to these objects as preferring role-interfaces.
* These tests are easier to set up, as they don't require setting up the state of the world prior to test invocation.
* Tests tend to be more coupled to implementation, and may be more difficult to refactor due to very specific requirements for message passing between collaborators.
* Fowler brings up a point where being a mockist means that your objects prefer to [Tell Don't Ask](https://pragprog.com/articles/tell-dont-ask). A nice side effect of TDA is you generally can avoid Demeter violations.

## In conclusion

In coming from a classic TDD background, I've oftentimes viewed mockist testing with some suspicion, particularly around how much work is involved to bring them about. Over the years, I've warmed up to the usage of mockist testing, but have not been diligent enough at doing pure driving TDD with mocks. In reviewing Fowler's comments, I'm intruiged at the possibilities of mockist TDD in affecting system design, particularly in their natural inclinations toward [role interfaces](http://martinfowler.com/bliki/RoleInterface.html). I look forward to trying pure mockist TDD in a future project.

#### Further reading:
* [Dan North: "Introducing BDD"](http://dannorth.net/introducing-bdd/)
* [James Golick: "On Mocks and Mockist Testing"](http://jamesgolick.com/2010/3/10/on-mocks-and-mockist-testing.html)
* [OOPSLA 2004: "Mock Roles, Not Objects"](http://jmock.org/oopsla2004.pdf)
* [Amazon: "Growing Object Oriented Software, Guided by Tests"](http://www.amazon.com/Growing-Object-Oriented-Software-Guided-Tests/dp/0321503627)
