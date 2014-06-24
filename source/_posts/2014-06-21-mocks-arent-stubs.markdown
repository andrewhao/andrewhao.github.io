---
layout: post
title: "Mocks aren't stubs, revisited"
date: 2014-06-21 17:55
comments: true
categories: 
- TDD
- mocks
- stubs
---
With the famed "TDD is dead" debate around the Rails community largely
coming to an end, I found myself referencing Martin Fowler's article,
[Mocks Aren't
Stubs](http://martinfowler.com/articles/mocksArentStubs.html) a good
deal, trying to make sense of it in terms of how I write tests and code.

In this post I'm going to talk about mocking and stubbing and their
roots, paraphrase Fowler in an attempt to explain their differences, and
walk through a couple of code examples. In each case, I'm going to
attempt to build this example out in Ruby and RSpec 3.

## What they are

### Mocks

Mocks are fake objects that verify that they have received messages. In
RSpec, we traditionally use the `double` or the `mock` methods to create
these objects.

Traditionally, you may have seen this in your code:

```ruby
api_client = double('api client')
expect(api_client).to receive(:list_book!).with(book).and_return(true)
subject.do_something!
```

What's happening here? RSpec creates a fake object that will verify
that, after the test case executes, it has received the `:list_book!`
message with the correct arguments.

### Stubs

Let's take a look at a stub -- a stub is a fake object that is set up to
respond to a certain message with a pre-canned response, each time. It
is identical to a mock, sans the verifying behaviors. So if I were to
write this same api client code again with a stub, it would look eerily
similar:

```ruby
api_client = double('api client')
allow(api_client).to receive(:list_book!).with(book).and_return(true)
expect(subject.do_something!).to change(subject, :requests).by(1)
```

Okay, so what's the big deal here? My test case still passes. Note that
I had to change my code to focus its expectation on the `subject`'s
state instead of the `api_client`.

This is the key difference between the mock and the stub -- the focus of
the test when using the mock is the subject under test (SUT). The focus
