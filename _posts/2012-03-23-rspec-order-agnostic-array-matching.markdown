---
author: andrewhao
comments: true
date: 2012-03-23 18:09:40
layout: post
slug: rspec-order-agnostic-array-matching
title: RSpec order-agnostic array matching
wordpress_id: 1140
categories:
- Andrew 2.0
- Rails
- Ruby
tags:
- matcher
- rspec
- ruby
---

What's that? [You want to write an expectation for an array but your method returns the Array in a nondeterministic ordering](http://stackoverflow.com/questions/2978922/rspec-array-should-another-array-but-without-concern-for-order)?

Simple. Write:

    
    my_method.should =~ <my_expectation>


See the [source](https://github.com/dchelimsky/rspec/blob/master/lib/spec/matchers/match_array.rb).
