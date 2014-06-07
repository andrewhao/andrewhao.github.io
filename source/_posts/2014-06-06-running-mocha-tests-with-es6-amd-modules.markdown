---
layout: post
title: "Running Mocha tests with ES6/AMD modules"
date: 2014-06-06 15:11
comments: true
categories: 
- TDD
- AMD
- requirejs
- mocha
- node
- testing
---

In one of my personal projects ([Chordmeister](https://github.com/andrewhao/chordmeister)), I've been trying to
upgrade the code to be written in [ES6 modules](http://wiki.ecmascript.org/doku.php?id=harmony:modules#export_declarations) and transpile down to AMD modules with Square's [very excellent es6-module-transpiler project](https://github.com/square/es6-module-transpiler).

Since I've already [updated](https://github.com/andrewhao/hendrix) an Ember app of mine to try ES6, I figured it was high time to do it on another project.

## Sorry Coffeescript, but I'm moving on.

First problem: Coffeescript seems indecisive with respect to ES6
support. In order to support `import` or `export` keywords, I had to
wrap the statements in backticks, making the code look like this:

```coffeescript
`import ClassifiedLine from "chordmeister/classified_line"`
class Parser
  # Implementation

`export default Parser`
```

Except this wasn't being picked up by es6-module-transpiler, since
Coffeescript wraps the entire declaration in a closure: I was
finding myself having problems compiling from Coffeescript -> ES5 JS -> ES6 JS.

```javascript
define("chordmeister/parser",
  [],
  function() {
    "use strict";
    (function() {
      // Oops, I wasn't transpiled!
      import ClassifiedLine from 'chordmeister/classified_line';
      var Parser;
      Parser = (function() {
        // Implementation
      }
      )();
      // Oops, I wasn't transpiled!
      export default Parser;
      })()
  });

```

So the first call: ditch Coffeescript. Write this in pure ES6.

```javascript
import ClassifiedLine from 'chordmeister/classified_line';
var Parser;

Parser = (function() {
  // implementation
})();

export default Parser;
```

Which transpiled nicely to:

```javascript
define("chordmeister/parser", 
  ["chordmeister/classified_line","exports"],
  function(__dependency1__, __exports__) {
    "use strict";
    var ClassifiedLine = __dependency1__["default"];
    var Parser;
    Parser = (function() {
      // Implementation
    })();
    __exports__["default"] = Parser;
    });
```


## Next up: adding AMD support in Mocha

Okay, so we need to set up a few things to get Mocha playing well with RequireJS, the AMD loader.

Our plan of attach will be to leverage the generated AMD modules and load our tests up with them. We have the benefit of being able to specifically inject dependencies into our test suite.

The tricky parts will be:

### Set up the Mocha index.html runner

Install `mocha`, `require.js`, and `chai` via `bower`, then plug them into the harness:

``` html test/index.html
<!doctype html>
<html>
<head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1">
    <title>Mocha Spec Runner</title>
    <link rel="stylesheet" href="../bower_components/mocha/mocha.css">
</head>
<body>
    <div id="mocha"></div>
    <script src="../bower_components/mocha/mocha.js"></script>
    <script src="../bower_components/chai/chai.js"></script>
    <script data-main="test_helper" src="../bower_components/requirejs/require.js"></script>

</body>
</html>

```

Note the references to `data-main="test_helper"`, which is require.js's way of determining its entry point after it loads.

### Set up a test runner.

``` javascript test/test_runner.js
// Configure and set up the tests.
require.config({
  baseUrl: "../build/"
})

// Load up the files to run against
var specs = [
  'chordmeister/chord_spec.js',
  'chordmeister/song_spec.js',
  'chordmeister/parser_spec.js',
  'chordmeister/classified_line_spec.js',
];

// Start up the specs.
require(specs, function(require) {
  mocha.setup('bdd');
  expect = chai.expect;
  // Why? Some async loading condition? Is there a callback I should be hooking into?
  setTimeout(function() {
    mocha.run();
  }, 100);
});
```

You'll notice that I was having synchonicity issues between spec suite load and `mocha.run()`. Throwing everything back a few hundred ms seemed to have done the fix.

## AMD gotchas

Pay attention to the `default` parameter that the module exports with. This is important to remember since native ES6 will allow you to directly import it with its native syntax:

```javascript
import Parser from "chordmeister/parser"
```

But if you're using RequireJS/AMD modules, you'll need to explicitly call out the `default` namespace from the required module, so like so:

```javascript
require(["chordmeister/parser"], function(parser) {
  Parser = parser.default;
  new Parser() // and do stuff.
});
```

Let me know if you have any questions!