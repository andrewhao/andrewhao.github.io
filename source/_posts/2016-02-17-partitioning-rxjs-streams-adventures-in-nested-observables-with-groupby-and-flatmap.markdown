---
layout: post
title: "Partitioning RxJS streams: adventures in nested Observables with groupBy() and flatMap()"
date: 2016-02-17 22:47
comments: true
categories:
- RxJS
- Observables
- Streams
---

One of the confusing aspects about working with streams is diving into
Rx operators that take a stream and fan out into multiple streams.

Is your head exploding yet?

## The problem:

Let's dive into a problem I ran into while working on a personal
project:

The task at hand is to take a list of GPS moving point data and
partition the group data into multiple clusters of points, some of which
are useful, and some which aren't.

The weapon of choice is the [RxJS groupBy() function](http://reactivex.io/documentation/operators/groupby.html),
which groups like, contiguous stream values based on a key value you
define.

![Image of groupBy() at work, with marbles.](http://reactivex.io/documentation/operators/images/groupBy.c.png)

OK. Easy enough. So my implementation looked something like this:

```js
gpsPointStream
.groupBy((point) => point.velocity)
```

Now at this point, we have to stop and scratch our heads. OK, so now my
stream is... many streams? What do I do here? All of my operators I'm
familiar with in RxJS have to do with operations on a single stream.

## Never fear, `flatMap()` to the rescue.

So the story turns to our hero `flatMap()`, which as it turns out is
specifically tuned to deal with issues of dealing with multiple streams.

```js
gpsPointStream
.groupBy((point) => point.velocity)
.flatMap((group) => {
  return group.reduce((acc, g) => g.elapsedTime + acc, 0)
});
```

What just happened here?

I specified a merging function for the `flatMap()` stream, which
performed the `reduce()` aggregation on my group before merging the
stream back into the main stream.

And thus continues the journey of learning RxJS patterns and the toolkit
you're offered. The easy things are easy, but then the moderate things,
sometimes, are less obvious and require some digging.

## Further reading:

* http://blogs.microsoft.co.il/iblogger/2015/08/11/animations-of-rx-operators-groupby/



