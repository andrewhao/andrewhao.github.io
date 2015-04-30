---
layout: post
title: "Building GPX stats through FRP principles with Bacon.js"
date: 2015-04-30 12:54
comments: true
categories: 
- functional reactive programming
- javascript
- node
---
With my current fascination with [tracking workouts and location-based-activities](http://github.com/andrewhao/stressfactor), I have been interested in how I might be able to rewrite some of my stats logic with FRP principles.

### What is FRP?

FRP, or Functional Reactive Programming, is often defined as "functional programming over values that change over time". It uses functional composition for streams of data that may appear in an infinite stream of data for some far indeterminate future - these types of use cases are served well by FRP which ["(simplifies) these problems by explicitly modeling time"](http://en.wikipedia.org/wiki/Functional_reactive_programming).

### GPS - your location, varied over time.

A great application of this would be a workout. Let's say I wanted to build an app that received realtime updates on a person's position. Say the app was a Node server that received this JSON blob from a web API as a location update:

```json
{ 'lat': 29.192414,
  'lon': 148.113241,
  'ele': 122.1,
  'time': '2015-04-18T13:54:56Z' }
```

Say that some time later, the API receives this JSON blob:

```json
{ 'lat': 29.192424,
  'lon': 148.113251,
  'ele': 123.1,
  'time': '2015-04-18T13:55:26Z' }
```

So we have these data points, that the user has moved `+0.00001` latitude points and `+0.00001` longitude points, climbing a total of `+1.0` meters, over a period of `30` seconds.

#### Exercise: Get my instantaneous velocity

If we performed this imperatively, we would write it something like this:

```javascript
var locations = [{ /*json*/ }, { /*json*/ } /*, ...*/];
var last = locations[locations.length-1];
var secondToLast = locations[locations.length-2];
var timeDelta = last.time - secondToLast.time;
var distanceDelta = getDistance(last.lon, last.lat, secondToLast.lon, secondToLast.lat);
var velocity = distanceDelta / timeDelta;
console.log(velocity);
```

With FRP, it might look more like this:

```javascript
var locationStream = [{ /*json*/ }, { /*json*/ } /*, ...some JSON objects that might appear in the future */];
locationStream.slidingWindow(2)
              .map(function(pairs) {
                var timeDelta = pairs[1].time - pairs[0].time;
                var distanceDelta = getDistance(pairs[1].lon, pairs[1].lat, pairs[0].lon, pairs[0].lat);
                return distanceDelta / timeDelta
              })
              .onValue(function(velocity) {
                console.log(velocity);
              });
```

There is a key difference that is not easily demonstrated here - that the former imperative example requires that all JSON arrays be materialized at once - via db query, in-memory store, etc. It doesn't account for change in time.

However, the latter functional example accounts for changing values of time as they appear over the stream - as soon as a new value shows up in the stream, the velocity is changed instantly.

### Some more location-based experiments: rxlocation

I wrote up a library to parse various facts from a changing stream of GPS events, from instantaneous velocity, average velocity, moving/stopped status, etc.

I investigated different reactive frameworks, mainly [RxJS](https://github.com/Reactive-Extensions/RxJS/) and [Bacon.js](https://github.com/baconjs/bacon.js/). My takeaways were that RxJS does everything and the kitchen sink, but I got lost trying to reconcile Node streams with RxJS cold streams. Bacon.js just seemed to work for me, out of the box. I'm still learning, so I hope to have a better understanding of the core issues here.

You can check it out here: [rxlocation](https://github.com/andrewhao/rxlocation).