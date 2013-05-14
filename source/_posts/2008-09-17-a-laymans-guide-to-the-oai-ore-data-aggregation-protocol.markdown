---
author: andrewhao
comments: true
date: 2008-09-17 01:37:12
layout: post
slug: a-laymans-guide-to-the-oai-ore-data-aggregation-protocol
title: A layman's guide to the OAI-ORE data aggregation protocol
wordpress_id: 880
categories:
- Andrew 2.0
- Geek
- Information Architecture
tags:
- atom
- berkeley
- computer-science
- Information Architecture
- internet
- nerds
- oai-ore
- rdf
- School
- Web
---

In Spring 2008, I took an information architecture course in the [UC Berkeley School of Information](http://ischool.berkeley.edu), taught by [Erik Wilde](http://dret.typepad.com) (disclaimer: I dropped the course after a month because of schedule conflicts). An interesting part of my research for that class involved investigating the nascent [Object Reuse and Exchange (OAI-ORE)](http://www.openarchives.org/ore/) protocol, a way of organizing groups of resources across the Web. I'm going to try to describe in laymen's terms what I learned from my research and explain why it's significant.


### Group things together on the Web


Let's say you're looking for a book online. You'll hit up JStor or Google Books and browse through the pages of that book you found. Easy enough, right?

In reality, the _physical, electronic representation_ of the book online is actually more than just "that book on JStor" or "http://www.jstor.com/book". It's actually a collection of images on a Web server. We need a uniform way to descibe that jStor' specific online version of "War and Peace" is actually comprised of "page1.jpg", "page2.jpg", et cetera.

As another example, let's say that the local public library wants to create a digital representation of its materials on Jimi Hendrix. Well, the library's got Jimi Hendrix CDs, Jimi Hendrix books, Jimi Hendrix magazine articles, and so on. With the OAI-ORE protocol, we can create a digital "Jimi Hendrix @ Your Local Public Library" object that lists out each CD, book and magazine article contained in the library.

Let's say on top of that, a certain Jimi Hendrix biography references "Little Wing" on pages 12, 141 and 254. This "Jimi Hendrix" object is capable of encoding that reference from those pages to "Little Wing" on the "Axis: Bold as Love" album in the library.


### Why even have a description framework?


As the Web gets more connected and as we're seeing the rise of interoperable Web services, it becomes increasingly important to have a standard resource description language. Various protocols (such as RDF) have built a foundation on which to describe relationships between electronic resources.

Think of OAI-ORE as a way of organizing information for re-use by others. If every library created its own "Jimi Hendrix" resource object and made them accessible to others, we could potentially create programs and Web services to collect all "Jimi Hendrix" objects from across the globe and reconstruct the ultimate, canonical Jimi Hendrix electronic resource library. We could note which books made references to "Little Wing." We could view "Little Wing" as an WMA file, an MP3 file, or as lyrics.

The key is that by standardizing a description language, we **enable programmatic ways to collect and synthesize data** that can potentially add to the value of the world's sum knowledge.


### Resource maps & implementation formats


OAI-ORE is a specification that defines relationships between resources through an object called the _resource map_. The resource map is a resource file enumerating a collection of resources and describing the relationships between each resource. Each resource map has its own URI, so it can be discoverable and accessible.

There is no standard file format to encode resource maps. Currently, the OAI-ORE working group has suggested the following description formats (HT: [Univ. of Queensland](http://www.natlib.govt.nz/downloads/IT19-Hunter-OAI-ORE-Oct30.ppt)):



	
  * [RDF](http://www.w3.org/TR/REC-rdf-syntax/) (Resource Description Framework)

	
  * [Atom](http://en.wikipedia.org/wiki/Atom_(standard)) syndication format

	
  * [YADS](http://nurture.nature.com/yads)

	
  * [TriX](http://www.w3.org/2004/03/trix/) (RDF triples in XML)


The [recommended method of describing a resource map is in RDF/XML](http://www.openarchives.org/ore/0.9/rdfxml). I'm not very familiar with the RDF syntax, however, and the TriX format, which affords us RDF triples that can easiliy express relationships between objects and resources, seems to make more sense to me.. However, there is a [complete Atom implementation of the protocol](http://www.openarchives.org/ore/0.1/atom), used when syndication is preferred.

I'd definitely refer the reader to the [Univ. of Queensland's OAI-ORE PDF overview](http://www.natlib.govt.nz/downloads/IT19-Hunter-OAI-ORE-Oct30.ppt) for a broader overview of the protocol and its potential implementations.


### The nitty-gritty




> **Collections**: Multiple resources can be grouped together, but only via an "aggregate object" or a "collection", which maintains pointers to each object in a collection.




> **Links**: Links describe relationships between objects in a resource map, such as "has-part" ("War & Peace "-has-part-"page 2") or "derived-from" ("Radiohead - 15 Step Remix"-derived-from-"Radiohead - 15 Step"). Actual relationships depend on selected vocabularies and namespaces (meaning you can change the link relationships to be whatever you need them to be). There is a [list of default namespaces on the spec](http://www.openarchives.org/ore/0.9/primer.html#Namespaces).




### In conclusion


I took a high-level and somewhat abstract tack in describing the protocol, and would direct you to the [OAI-ORE Primer](http://www.openarchives.org/ore/0.9/primer.html) for more implementation specifics. Apologies to those of you who were completely lost in this discussion.

I'm excited about developing standards like these because resource mapping protocols such as OAI-ORE are an instrumental step in connecting resources, services and information on the Web. They'll help us "connect the tubes" and create the infrastructure needed to link the world's information together. Now that's pretty cool.
