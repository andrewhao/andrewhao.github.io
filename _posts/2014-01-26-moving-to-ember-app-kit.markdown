---
layout: post
title: "Moving to Ember App Kit"
date: 2014-01-26 23:32
comments: true
categories: 
- ember
- ember app kit
- development
- programming
---
I've noticed a bit of the buzz around [Ember App Kit](https://github.com/stefanpenner/ember-app-kit)
recently and decided to move Hendrix, my music management app, over from
a [Yeoman](http://yeoman.io/)-generated Ember app to EAK with all its
bells and whistles.

### What's the difference?

Well on the surface, the two frameworks aren't very different. The
standard Yeoman build tool sets you up with Grunt and Bower, which is
what EAK provides you out of the box. The cool stuff happens when you
dive under the hood: ES6 module transpilation and an AMD-compatible
Ember Resolver, built-in Karma integration and a built-in API stub
framework for development and test environments.

### The joys of modules

What I didn't realize was that compiling to ES6 modules required that my
filenames be renamed exactly how the modules were going to be placed,
with the extra caveat that resource actions needed to live in their own
directories. Recall that in the old way of doing things with globals and
namespaces, you could get away with throwing a route file like this in
your app directory:

```
routes/
  songs_index_controller.js
```

And inside:

```javascript
MyApp.SongsIndexRoute = Ember.Route.extend({
  //...
});
```

In EAK's world, you need to nest the file under the `songs/` directory,
and strip the type from the filename, like so:

```
routes/
  songs/
    index.js
```

Inside the file, you assign the function to a variable and let it be
exported in the default namespace.

```javascript
var SongsIndexRoute = Ember.Route.extend({
  //...
});
export default SongsIndexRoute;
```

### File name matters

The [new Ember resolver](https://github.com/stefanpenner/ember-jj-abrams-resolver/) 
loads modules in a smart way -- according to how the framework
structures resources, controllers and their corresponding actions. So
visiting `#/songs` from my app caused the app to look up and load
`appkit/routes/songs/index`. What I didn't realize was *this module must
live at a very specific place in the file directory structure*.
I realized that I left the module type in the file name the first time
around, like this:

```
routes/
  songs/
    index_route.js
```

There are no types in the module names -- or the filenames, for that
matter. I had not realized this (I'm also an AMD newbie) -- so I had
left my files un-renamed as `songs_index_route`, which meant that
the module loader had stored the SongsIndexRoute module under
`appkit/routes/songs/index_route`, but was doing a route lookup through
the Resolver for: `appkit/routes/songs/index`. Renaming the file to:

```
routes/
  songs/
    index.js
```

did the trick.
