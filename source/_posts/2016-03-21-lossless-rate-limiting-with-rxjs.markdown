---
layout: post
title: "Lossless rate limiting with RxJS"
date: 2016-03-21 13:09
comments: true
categories:
- RxJS
- FRP
- Reactive programming
- Rate limiting
---

Much of RxJS involves working with [backpressure](http://reactivex.io/documentation/operators/backpressure.html) - how to reconcile streams that emit/process data at different rates, without overloading the system. Much of that model is built with lossy handling in mind - it makes sense that when your system is under duress, that you design your streams to degrade gracefully (e.g. drop certain events, or rate limit them by chunking into windows, etc).

However, there are times when it is appropriate to have a lossless approach to backpressure - e.g., to store every chunk of data that comes through a stream in memory, and not drop things. These use cases may come about when:

* You have a short-lived, or bounded set of data you know will come over the pipe. You understand the bounds of the data that will ever come over the pipe.
* You have a processing script you want to run, which is not part of a large system.
* You have a honkin' large system that can handle the load.

In my case, I had a script that called the Google Geocoding API for a set of GPS coordinates. Now for a set of several hundred coordinates, I would end up calling the API several hundred times all at once with this naive implementation:

``` javascript
// address$: [ "1234 Widget Way, Promiseland, WV" ] -- [...] -- [...]
const geocoded$ = addresses$
.flatMap(address => Rx.Observable.fromPromise(callGoogleGeocodingService(address)))
// geocoded$: [ { latitude: 89.99, longitude: 90.00, ... } ] -- [...] -- [...]
```

I searched all over for a lossless throttling mechanism, but all I could find was references to RxJS's lossy [throttle]() behavior.

Other frameworks, like [Bacon.js's bufferingThrottle()](https://github.com/baconjs/bacon.js/#observable-bufferingthrottle) and [Highland.js ratelimit()](http://highlandjs.org/#ratelimit) seemed attractive. Where was RxJS's equivalent?

Thanks to a [helpful StackOverflow post](http://stackoverflow.com/questions/34955842/rate-limiting-http-calls-made-by-rxjs), I found the answer: the use of [concatMap()](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/concatmap.md) and [delay()](https://github.com/Reactive-Extensions/RxJS/blob/master/doc/api/core/operators/delay.md) forces the incoming stream to execute serially over artificial time delayed streams.

```javascript
const geocoded$ = addresses$
.concatMap(address => Rx.Observable.just(address).delay(TIME_INTERVAL))
.flatMap(address => Rx.Observable.fromPromise(callGoogleGeocodingService(address)))
```

Thanks to:

* http://stackoverflow.com/questions/34955842/rate-limiting-http-calls-made-by-rxjs
* http://stackoverflow.com/questions/30876361/rxjs-rate-limit-requests-per-second-and-concurrency?rq=1
