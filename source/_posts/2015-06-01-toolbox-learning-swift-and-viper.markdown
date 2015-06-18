---
layout: post
title: "Toolbox: learning Swift and VIPER"
date: 2015-06-01 17:04
comments: true
published: false
categories:
- swift
- viper
- ios
---

* I had trouble importing nested source code into XCode before realizing that I
needed to import the folder with corresponding Groups. This is done by
clicking the checkbox "Create Groups for any Added Folders"

Reference:
https://developer.apple.com/library/ios/technotes/iOSStaticLibraries/Articles/configuration.html

* Since there is no way to do method swizzling in Swift, there are no real easy ways to do mocking/stubbing the way we used to do so in Ruby. 
* Using `vipergen` and `boa` scaffolding generators helped me understand the concepts behind the view.
* Build a VIPER module, but don't build it all at once. Just focus on the Presenter-Interactor-Wireframe component, or the DataStore-Entity-Interactor component. This will keep your head from exploding.
* Dude. I miss vim. Alcatraz + xvim helped a little...
* xcodebuild + xcpretty + Guard-shell == some sort of CI feedback loop.
* Manually creating mocks in Swift = kind of painful. If you override (subclass) a NSObject in Swift, you must provide it with the `@objc` pragma, otherwise it throws a segfault error (http://stackoverflow.com/a/30530308/993929)
