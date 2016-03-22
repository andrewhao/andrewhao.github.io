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
partition the group data into multiple clusters of points, count up each
group, then return the aggregate stats. As a cyclist is moving, I want
to know how often they are moving at that specific velocity (speed).

Our weapon of choice is the [RxJS groupBy() function](http://reactivex.io/documentation/operators/groupby.html),
which groups like stream values based on a key value you define.

[![Image of groupBy() at work, with marbles.](http://reactivex.io/documentation/operators/images/groupBy.c.png)](http://reactivex.io/documentation/operators/groupby.html)

OK. Easy enough. So my implementation looked something like this:

```js
gpsPointStream
.groupBy((point) => point.velocity)
```

The supplied `(point) => point.velocity` function determines the `key`
value for the supplied event, which then 1) creates a new Observable
sequence for that specific `key` value, if it doesn't exist, or 2)
assigns your event to an existing Observable sequence.

Let's illustrate:

```
src:     -- { velocity: 0 } ---------- { velocity: 0.1 } ----------------------------------- { velocity: 0 } -->
groupBy: -- [{ Observable key: 0 }] -- [ { Observable key: 0 }, { Observable key: 0.1 } ] -- [ { Observable key: 0 count: 2 }, { Observable key: 0.1 } ] -->
```

## Never fear, `flatMap()` to the rescue.

So the story turns to our hero
[`flatMap()`](http://reactivex.io/documentation/operators/flatmap.html), which as it turns out is
specifically tuned to deal with issues of dealing with multiple streams.

[![Marble diagram for flatMap](http://reactivex.io/documentation/operators/images/flatMap.c.png)](http://reactivex.io/documentation/operators/flatmap.html)

`flatMap` will take a supplied function as its argument, which is the
operation to apply to each argument within the supplied stream.

```js
gpsPointStream
.groupBy((point) => point.velocity)
.flatMap((group) => {
  return group.scan((h, v) => h + 1, 0)
              .zip(Observable.just(group.key))
});
```

```
src:     -- { velocity: 0 } ---------- { velocity: 0.1 } ----------------------------------- { velocity: 0 } ---->
groupBy: -- [{ Observable key: 0 }] -- [ { Observable key: 0 }, { Observable key: 0.1 } ] -- [ { Observable key: 0 count: 2 }, { Observable key: 0.1 } ] -->
flatMap: -- [ 1, 0 ] ----------------- [ 1, 0.1 ] ------------------------------------------ [ 2, 0 ] -->
```

What just happened here?

I specified a merging function for the `flatMap()` stream, which
performed the `scan()` counting aggregation on my group before merging the
stream back into the main stream. I threw in a `zip`, which annotated my
aggregate count value with a record of the group key (velocity) that
this value was computed for.

## Compare it to imperative

The equivalent of `groupBy`/`flatMap` in imperative programming is, quite
literally, just `_.groupBy()` and `_.flatMap()`. With a few key
differences. Here it is in [lodash](https://lodash.com/docs#groupBy):

```js
var grouped = _([ { velocity: 0 }, { velocity: 0.1 }, { velocity: 0 } ])
.groupBy((point) => point.velocity)

grouped.value()
// { 0: [ { velocity: 0 }, { velocity: 0 } ], 0.1: [ { velocity: 0.1 } ] }

var flatmapped = grouped.flatMap((v, k) => {
  return [ [v.length, k] ]
  })

flatmapped.value()
// [[2, "0"], [1, "0.1"]]
```

So in the end, the end result was the same with one crucial difference -
our Observable, reactive version was able to take intermediate accounts
into time and perform an intermediate calculation as data was flowing
in. This allowed us to generate an intermediate count for the "0" velocity
group.

## Takeaways

* When you want to fan out a stream into groups or partitions based on a
specific stream value, turn to
[`groupBy`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/groupby.md).
* When you have a need to combine a stream-of-streams, you want to look at
[`flatMap`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/selectmany.md). You may also consider looking at [`concatMap`](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/concatmap.md), a close cousin of `flatMap`.
* Reactive programming gives you more expressive abilities to reason
about time and event ordering. You just have to tilt your head a little
bit.

## Further reading:

* http://blogs.microsoft.co.il/iblogger/2015/08/11/animations-of-rx-operators-groupby/

__Update: 2016/03/22__

Updated typo where the `index` variable on a GroupedObservable was
changed to correctly be `key`.
