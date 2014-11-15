---
layout: post
title: "My own robot training buddy."
date: 2014-11-15 10:03
comments: true
categories: 
- open source
- software
- running
---
As an ultra runner, I am really into the mountains. As a software engineer, I'm really into data. So naturally, I'm interested in the intersection of both.

I've particularly been interested in how systems like [Strava](http://www.strava.com) work, especially when they quantify what is known as a "Suffer Score", a single number denoting the amount of training stress (a.k.a. suffering) you put yourself through in a workout.

How does a track workout compare to a long day on the trails? Which is tougher: a 5m tempo road run in and around my neighborhood, or a tough 2m climb into a local regional park?

## Data in...

I first attacked the problem of getting data off of my phone. I record my GPX tracks in [Runmeter](http://runmeter.com/), a fantastic iPhone application with all sorts of metrics and data export capabilities. What I wanted was a seamless way to get the data off my phone without fuss after a hard workout.

The application has a nifty feature in which it can automatically send an email to an email address after a workout is completed.

I wrote an [email ingester, Velocitas](https://github.com/andrewhao/velocitas), with the help of [Cloudmailin](http://cloudmailin.com/), which fires off a POST request to the Node application. Velocitas does the following:

* `curl`s and downloads the GPX link embedded in the email.
* Saves the GPX file to a linked Dropbox account.
* Republishes the GPX file to a linked Strava account.

## Deriving the Training Stress Score

Next up: I wanted to do a quick and dirty implementation of the (run-based) Training Stress Score. [Stressfactor](https://github.com/andrewhao/stressfactor), a Ruby gem, is what came out of it.

It implements the rTSS as detailed in [this article](http://home.trainingpeaks.com/blog/article/running-training-stress-score-rtss-explained).

```ruby
(duration_seconds * normalized_graded_pace * intensity_factor) /
(functional_threshold_pace * 3600) * 100
```

Stressfactor is a higher-level toolbelt for deriving meaning from GPX tracks, so it, at the moment, attempts to calculate the stress score and grade adjusted pace.

The data still needs validation, so I'm eager to run it on my data set of GPX tracks from the past years.

## Generating reports

I'm working on this part right now -- I need to nicely display a report from my workout history in Dropbox and display per-GPX. I've started the project -- [Stressreport](https://github.com/andrewhao/stressreport).

## Some things I've learned and am learning

* The human body is complex, and cannot be [easily modeled](http://fellrnr.com/wiki/Modeling_Human_Performance) without sufficient data. That said, what I'm doing now may be sufficient for basic training data.
* The nature of parsing and generating higher-order stats from raw data may lend itself well to experimentation with functional languages. I'm interested in trying to reimplement Stressfactor in Scala, or a functional equivalent.
* Deploying all my apps on Heroku's free tier may actually be an interesting start to building a microservice architecture -- with the space limitations on Heroku, I'm forced to build new features on new services. Call it cheapskate architecture.