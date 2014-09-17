# Keeping Your Frontend JS Modular

This article will propose a method of organizing frontend javascript that promotes modularization & long-term maintainability. Similarly to our [CSS guidelines](/CSS/overview.md) it centers around tiny single-purpose files chained together using build tools.

## Browserify

Browserify is a node module that bundles javascript files together. It adds the ability to use `require` statements to pull both local files and _node modules_ into a single javascript asset. Here's some actual input and output from Browserify:

#### Input

Megaphone.js - notice the `module.exports` call.
```javascript
function Megaphone(message) {
  var self = this;
  self.message = message;

  self.speak = function() {
    console.log(self.message);
  };

  self.yell = function() {
    console.log( self.message.toUpperCase() );
  };
}

module.exports = Megaphone;
```

example.js (the main site file)
```javascript
var Megaphone = require('./Megaphone.js');

var megaphone = new Megaphone('hello');
megaphone.yell();
```

#### Output

```javascript
(function e(t,n,r){function s(o,u){if(!n[o]){if(!t[o]){var a=typeof require=="function"&&require;if(!u&&a)return a(o,!0);if(i)return i(o,!0);throw new Error("Cannot find module '"+o+"'")}var f=n[o]={exports:{}};t[o][0].call(f.exports,function(e){var n=t[o][1][e];return s(n?n:e)},f,f.exports,e,t,n,r)}return n[o].exports}var i=typeof require=="function"&&require;for(var o=0;o<r.length;o++)s(r[o]);return s})({1:[function(require,module,exports){
function Megaphone(message) {
  var self = this;
  self.message = message;

  self.speak = function() {
    console.log(self.message);
  };

  self.yell = function() {
    console.log( self.message.toUpperCase() );
  };
}

module.exports = Megaphone;
},{}],2:[function(require,module,exports){
var Megaphone = require('./Megaphone.js');

var megaphone = new Megaphone('hello');
megaphone.yell();
},{"./Megaphone.js":1}]},{},[2]);
```

The output starts with a definition of the `require` function, takes both files and wraps them into an object, and provides a key for which file to run when it is required. Browserify's options cover some interesting use cases, in particular optionally providing a source map (in development) so that the Chrome console behaves as if you are using the individual javascript files. In this case errors are reported at the line numbers of the original files!

## No Global Variables

Notice that the output above does not expose any global variables. A variable becomes limited to the scope of the file, just like in node. Using `module.exports` is the only way to escape this scope, forcing you to pay more attention to the modularity of your components.

There are only two situations where global variables are necessary:
  - They are well-formed abstract libraries (`jQuery`, `moment`, `lodash`, etc)
  - They need to be accessed outside of a closure _after pageload_ by inline scripts or other unrelated components

Within this structure you'll have to be explicit about what you attach to the window:

```javascript
window.megaphone = new Megaphone('I belong to the window.');
```

## Build Components & Export Patterns

This structure encourages you to build isolated components. Files should be small (< 200 lines) and very focused. When creating class-like components (like the fictional `Megaphone` above) the `module.exports` functionality allows you to export the function. For files that don't have obvious functions or objects to export– when binding jQuery events, for example– there are two options.

#### Exporting Files with jQuery Bindings

We could run through the jQuery bindings when the file runs:

```javascript
// bindEvents.js

$('.megaphone-button').click( function(e) {
  $('.megaphone-yell').show();
});
```

main site file:

```javascript
// site.js

require('./bindEvents.js');  // this causes the code to run
```

#### Or

We could wrap our bindings in a function, allowing them to be called only when they are needed:

```javascript
// bindEvents.js

function bind() {
  $('.megaphone-button').click( function(e) {
    $('.megaphone-yell').show();
  });
}

module.exports = bind;
```

Then our main site file can take on the role of controller:

```javascript
// site.js

$('.megaphone').exists( function() {
  require('./bindEvents.js')();
});
```

The advantage here is that each file does not need to know when to run, allowing us to keep all of our controller logic in one place. We can also pass arguments through the function if we need to give our bindings a little more context.