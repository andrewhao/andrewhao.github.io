---
layout: post
title: "Toolbox: learning Swift and VIPER"
date: 2015-06-01 17:04
comments: true
categories:
- swift
- viper
- ios
---

The following are some notes I'm compiling as I'm beginning a journey
down the rabbit hole, writing an app in Swift utilizing the [VIPER app development methodology](http://www.objc.io/issue-13/viper.html)

* I had trouble importing nested source code into XCode before realizing that I
needed to import the folder with corresponding Groups. This is done by
clicking the checkbox "Create Groups for any Added Folders"

  *Reference*: https://developer.apple.com/library/ios/technotes/iOSStaticLibraries/Articles/configuration.html

  Without doing this, the compiler was not able to build the project.

  * Since there is no way to do method swizzling in Swift, there are no real easy ways to do mocking/stubbing the way we used to do so in Ruby. Instead, this is forcing me to rely on plain old Swift structs. There are some simple ways to stub, but it ends up looking kind of awkward and very wiring-intensive like this:

```
class NewRidePresenterSpec: QuickSpec {
  override func spec() {
    describe("#startRecordingGpsTrack") {
      class MockInteractor: NewRideInteractor {
        var wasCalled: Bool = false

        @objc private override func startRecordingGpsTrack() {
          wasCalled = true
        }
      }

      var subject = NewRidePresenter()

      it("tells the interactor to start recording") {
        let mockInteractor = MockInteractor()
        subject.interactor = mockInteractor
        subject.startRecordingGpsTrack()

        expect(mockInteractor.wasCalled).to(beTrue())
      }
    }
  }
}
```

* Using the [`vipergen`](https://github.com/teambox/viper-module-generator) and [`boa`](https://github.com/team-supercharge/boa) scaffolding generators helped me understand the concepts behind the view.
* Tip: Build a VIPER module, but don't build it all at once. Just focus on the Presenter-Interactor-Wireframe component, or the DataStore-Entity-Interactor component. This will keep your head from exploding.
* Dude. I miss vim. [Alcatraz](http://alcatraz.io/) + xvim helped a little...
* xcodebuild + [xcpretty](https://github.com/supermarin/xcpretty) + Guard-shell == some sort of CI feedback loop.
* Manually creating mocks in Swift = kind of painful. If you override (subclass) a NSObject in Swift, [you must provide it with the `@objc` pragma, otherwise it throws a segfault error](http://stackoverflow.com/a/30530308/993929)
* You must contact CircleCI manually if you want to activate an iOS build (it's still in beta). What are some other good CI tools to use with iOS?
