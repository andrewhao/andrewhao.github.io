---
author: andrewhao
comments: true
date: 2012-04-27 17:27:17
layout: post
slug: speeding-up-rspeccucumber-feedback-times-without-sacrificing-coverage
title: Speeding up Rspec/Cucumber feedback times without sacrificing coverage
wordpress_id: 1155
categories:
- Rails
- Ruby
- Software
- TDD
tags:
- cucumber
- rails
- rspec
- ruby
- tdd
- test
---

**[Rocket Fuelled Cucumbers](http://www.slideshare.net/josephwilk/rocket-fuelled-cucumbers)**

View more [presentations](http://www.slideshare.net/) from [Joseph Wilk](http://www.slideshare.net/josephwilk)





One thing the Blurb devs have been discussing is how we can speed up our test feedback cycles without sacrificing coverage. There's some good tips (mainly Rails+Rspec/Cucumber) in the presentation such as:








	
  * Don't run all the tests when developing (tag your tests by function)

	
  * Parallelize, chunk tests over machines/cores using Testjour/[Specjour](https://github.com/sandro/specjour), [Hydra](https://github.com/ngauthier/hydra)

	
  * Don't run all the tests at once. Tests that never fail should nightly.

	
  * Instead of spinning up a browser for acceptance tests, can you use a js/DOM simulator (e.g. [envjs](https://github.com/thatcher/env-js) via [capybara-envjs](https://github.com/smparkes/capybara-envjs), or [celerity](https://github.com/jarib/celerity))



