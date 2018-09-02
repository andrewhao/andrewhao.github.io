---
layout: post
title: "JHipster &amp; Spring Boot for Rails developers"
date: 2016-10-14 14:24:39 -0700
comments: true
categories: 
- Java
- Spring Boot
- Liquibase
- Ruby on Rails
published: true
---

The first question you may be asking is - _why would I want to go from Rails to Java_?

Maybe you don't have a choice. Maybe you started a new job. Maybe you heard Java was the new, old hotness. In any case, you're a Ruby on Rails developer and you're staring Java in the face.

### The languages

First off, Rails and Java share similar philosophies.

You may argue that Java is the One True Language for old-fashioned Object-Oriented Programming. The existence of strong types lead to powerful expressions around OOP concepts like inheritance, polymorphism, and the like.

Java and Ruby share similar philosophies - in that Everything Is An Object. In Ruby, the `nil` value is modeled as a `NilClass`. In Java, objects abound everywhere.

### The frameworks

I can't speak from firsthand experience, but Java developers will tell you that developing Java apps in the early 2000s was like configuration soup. Everything was explicit and configured in XML.

Spring Boot arguable moves the state of the art in Java frameworks more towards' Rails' philosophies - that convention trumps configuration. It accomplishes this through _annotations_ - more on this later.

Rails was precisely so groundbreaking and exciting in 2005 because it was everything Java was not - terse, expressive, and unapologetically convention-driven. Where in Java, everything was explicitly traceable through system calls, Rails used magic methods of dynamically-defined methods, monkey-patching and big ol' global God Objects to accomplish its magic.

### Liquibase vs Active Record

In Active Record, database changes are called _migrations_.

Migrations are only run from migration files, and may optionally be generated from the CLI.

In Liquibase, these are called _changesets_, and the files are called _changelogs_.

These migrations are either generated from the Liquibase CLI, or there is a nifty tool that reads Hibernate persistence entities and generates a "diff" against a known database, writing the migration to a file.

Rails checks in a `schema.rb` file, encompassing the canonical definition of the DB schema. There is no such equivalent in Liquibase (\* I may be wrong).

**To be continued...**
