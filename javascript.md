# JavaScript

Use P'unk Ave's [ESLint configuration](https://www.npmjs.com/package/eslint-config-punkave) (based on JavaScript Standard Style) to lint all JavaScript with your editor or during a build process. Install [Atom's ESLint plugin](https://github.com/AtomLinter/linter-eslint).

## Table of Contents

1. [Front-end](#front-end)

## Front-end
<a name="visual-state"></a><a name="1.1"></a>
[1.1](#visual-state) Add and remove classes to manage visual state

> Why? Provides a separation of concerns.

```javascript
// Good
$foo.removeClass('is-active');
// Bad
$foo.hide();
```

<a name="data-attributes"></a><a name="1.2"></a>
[1.2](#data-attributes) Use data attributes as event handlers

> Why? Provides a separation of concerns.

```javascript
// Good
$('[data-foo]').removeClass('is-active');
// Bad
$('.foo').removeClass('is-active');
```

<a name="event-delegation"></a><a name="1.3"></a>
[1.3](#event-delegation) Use event delegation. Don't bind events to `body` unless you don't have a consistent outer node to bind to. Bind to an outer container for the in question component if possible. Always use a named function to bind to `body` or the component so it's easier to debug using dev tools.

> Why? Event delegation allows you to attach an event to a parent node and account for matching descendants if they exist now or in the future.

```javascript
// Outer component
const $component = $('[data-foo]').find('data-bar');

function fooBar () {
  console.log('foo');
};

$component.on('click', fooBar);
```

<a name="variables"></a><a name="1.4"></a>
[1.4](#variables) Always declare and cache variables. If the variable contains a jQuery object prefix the variable name with `$`. Always put your variable statements *at the start of a function*. Declaring a variable with `let` or `var` inside a "while" or "for" loop does not make a unique variable for every pass through the loop. And that will mess you up good when you start trying to access them in [closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures).

> Why? Performance and maintainability.

```javascript
// Good
const $body = $('body');
const $component = $body.find('[data-foo]');

$component.on('click', fooBar);

// Bad
$('body').find('[data-foo]').on('click', fooBar);
```

**[⬆ back to top](#table-of-contents)**


# NEEDS REVIEW!

## No littering

In browser-side JavaScript, do not litter the namespace with your own variables at "top level." This is likely to interfere with other code that is trying to do the same thing.

In most cases your code should at least be inside a jQuery "DOM ready" function, which gives you your own "scope" for variables. You should do that anyway because jQuery may not be ready to let you access things otherwise.

WRONG:

```javascript
var $coolThing = $('[data-cool-thing]');
setInterval(function() {
  twinkleCoolThing();
}, 100);
```

RIGHT (without Apostrophe):

```javascript
$(function() {
  var $coolThing = $('[data-cool-thing]');
  setInterval(function() {
    twinkleCoolThing();
  }, 100);
});
```

However, see "doing things when the page loads" for notes on the best way to do this with Apostrophe.

If you don't care about jQuery, you can declare an anonymous function, then just run it:

```javascript
(function() {
  // my code goes here
})();
```

This declares a function and calls it with no arguments, creating a separate namespace for your variables.

## lodash

The [lodash](https://lodash.com/docs) module is always loaded in the browser when you work with Apostrophe, and is also available via npm in node. Always use `lodash` to minimize bugs by avoiding repetitive code. `lodash` is preferred over similar ES6 features because it is consistently available on every platform.

WRONG-ISH:

```javascript
var i;
for (i = 0; (i < things.length); i++) {
  handleThing(things[i]);
}
```

RIGHT:

```javascript
var _ = require('lodash');

_.each(things, handleThing);
```

*What do you mean by wrong-ish?"* The "for" loop isn't actually incorrect, and there may be circumstances where it is preferred for performance. But these circumstances are rare. For the most part synchronous JavaScript code is never the source of our performance problems.

`lodash` can also solve many other problems. Some of the methods used most often are `find`, `map`, `filter`, `pick` and `pluck`.

**lodash is not for async programming!** See below for best practices on working with callback functions.

## Async

### Always be returning

Every time you call a function that takes a `callback` argument, use the `return` keyword.

Otherwise execution continues in your code, and you will probably run code you didn't want to run.

WRONG:

```javascript
if (condition) {
  doStuffThen(callback);
}
moreCodeHere();
alwaysRunningByAccident();
```

RIGHT:

```javascript
if (condition) {
  return doStuffThen(callback);
}
moreCodeHere();
runningOnlyIfConditionIsFalse();
```

The only time you should ever break this rule is when the order truly doesn't matter. 99% of the time this doesn't apply to you. Use `return`.

### When you're async, you're async all the way

If your function takes a callback, make sure you invoke that callback asynchronously.

WRONG:

```javascript
function handleTheThing(thing, callback) {
  if (thing.handledAlready) {
    // I already did it, let's skip out!
    return callback(null);
  }
  // Do some real async work
  return db.insertThing(thing, function(err) {
    thing.handledAlready = true;
    return callback(err);
  });
}
```

RIGHT:

```javascript
function handleTheThing(thing, callback) {
  if (thing.handledAlready) {
    // I already did it, let's skip out!
    return setImmediate(callback);
  }
  // Do some real async work
  return db.insertThing(thing, function(err) {
    thing.handledAlready = true;
    return callback(err);
  });
}
```

Spot the difference? `return callback(null)` is *not safe if your code has not returned at least once already.* Basically: it is not safe if you haven't talked to a database, invoked `request`, or called some other async function.

"Why?" Because every time you call a function, JavaScript has to remember where you called from on a "stack." And if you never return, but keep calling deeper and deeper, the "stack" eventually crashes. For instance, the first version of `handleTheThing` will probably crash if you pass it to `async.eachSeries` with an array of 2000 things to process.

*setImmediate is always available in the browser when working with Apostrophe. If you're working in the browser without Apostrophe, use a shim, or write: `setTimeout(callback, 0)`*

### Always use the async module for async things

The async module solves all the terrible problems you'll encounter with callback-based functions. Learn it, love it, use it.

### Never use the async module for purely-synchronous things

If there are no callbacks (things that get called back **later**) in the code, don't use the async module. Use lodash where appropriate.

#### async.series

When you're working with functions that take callbacks, but you want things to happen in a certain order, it's easy to write confusing code. The async.series method exists to prevent this problem.

*The async module is always available in browser-side JavaScript in Apostrophe. In node you `require` it.*

Let's say we want to do three things in a sequence.

WRONG:

```javascript
doSomethingThen(function(err) {
  // handle err, then...
  doSomethingElseThen(function(err) {
    // handle err, then...
    doYetAnotherThingThen(function(err) {
      // handle err, then...
    });
  });
});
```

RIGHT:

```javascript
var async = require('async');

return async.series([
  doSomethingThen,
  doSomethingElseThen,
  doYetAnotherThingThen
], function(err) {
  // everything succeeded, unless err was set
});
```

ALSO RIGHT:

```javascript
return async.series({
  something: function(callback) {
    doSomethingThatNeedsArguments(one, two, callback);
  },
  somethingElse: function(callback) {
    doSomethingElseThatNeedsArguments(three, callback);
  }
}, function(err) {
  // hooray (if no err)
});
```

"What about `async.parallel`?" Don't. Keep it simple and run things in series. Unless you know for a fact that your functions never, ever depend on each other's work and your database won't mind all the extra simultaneous queries, times as many people as are looking at the website, there's really no reason to go there.

#### async.eachSeries

What if you have an array of objects that all need to be processed by a function that takes a callback? Process them one at a time with `async.eachSeries`, so that you know when your code has completed, and you don't overwhelm your database with queries.

WRONG:

```javascript
var i;
for (i = 0; (i < things.length); i++) {
  doSomethingToThing(i, function(err) {
    // Now what? Houston, we have a problem
  });
}
```

RIGHT:

```javascript
return async.eachSeries(things, function(thing, callback) {
  return doSomethingToThing(thing, callback);
}, function(err) {
  // if (!err) then everything was processed successfully
});
```

*Yes, I could have passed `doSomethingToThing` directly to `eachSeries`, but I wanted you to see what its arguments look like.*

*"What about async.each? Parallel is fast!"* Do you know exactly how many items could be in the array? Do you know how many queries your database will accept at once?

When you're coding a web app, you get plenty of parallelism from multiple people accessing it. You don't need to create extra parallelism just in handling a single request.

However, `eachLimit` is useful if you know you can handle a reasonable amount of parallelism. Especially in a command line task, where you know you won't have six copies running.

ALSO ACCEPTABLE:

```javascript
return async.eachLimit(things, 4, doSomethingToThing, function(err) {
  // All finished if no err. Up to 4 were processed at once
});
```

Here we're processing up to 4 things at a time. A good technique if you know you have, let's say, enough RAM to process four image imports at once. (Test it. Twice. Or just use `eachSeries`.)

## Using ES6 features

Should be avoided unless universal in browser versions supported by a project (probably not). Our clients need to support a variety of browsers and browser versions. Instead, *learn lodash* and use its features to avoid cross-browser gotchas. `lodash` is never the worst choice, and surprisingly fast. Often faster than ES6 built-ins anyway.

*ES6 refers to newer versions of JavaScript syntax not universally supported by IE9+.*

## Object-oriented programming

In A2 0.6, we use [moog](https://github.com/punkave/moog) for OOP in the browser, and [moog-require](https://github.com/punkave/moog-require) to provide OOP for "Apostrophe modules" on the server. But these are just more sophisticated versions of the "self pattern" explained below, so you should definitely read on before reading up on them.

### The "self pattern" at P'unk Avenue

There are many object-oriented programming styles in JavaScript, because the language provides a set of tinkertoys to create different styles of OOP and doesn't lay down many rules or patterns.

Most styles do depend on the `this` keyword, which is basically broken for asynchronous programming.

Our house style avoids this problem at a small cost in performance. We've never seen that matter in practice, even with tens of thousands of objects in play.

It's often described as the "self pattern." It's also a form of "concatenative inheritance."

Here's a simple "class" following the "self pattern:"

```javascript
function Dog(options) {
  var self = this;
  self.name = options.name;

  self.bark = function() {
    console.log(self.name + ' barked.');
  };
}

var dog = new Dog('spot');
dog.bark();
```

Key things going on here:

* The methods of the object live inside the constructor function, `Dog`, so they can always see `self` and its value never changes for this object.

* The constructor takes one argument, `options`, which is an object. You add properties as you need them. This makes it easier to extend the code without bc breaks (*backwards compatibility*).

* The `this` keyword is used exactly once, to capture it in `self`, a lexically scoped variable that always refers to the same thing, *even in a callback function or jQuery event handler*. And we never, ever touch `this` again. Learn to love `self`, as the lady sings.

* You can have "private properties" of the class just by using `var` statements in the constructor. You can also have "public properties" by setting properties of `self`.

### Subclassing

Here's a subclass that extends Dog:

```javascript
function Doge(options) {
  var self = this;
  Dog.call(self, options);

  self.meme = function() {
    console.log('such oop very code');
  };
}

var doge = new Doge('doge');
doge.meme();
doge.bark();
```

Key points:

* The subclass has to call the parent class constructor function. The `call` method invokes `Dog` with `this` set to the same object as your new `Doge`. It is very rare for us to have to use `call` or `apply` anywhere else.
* We can override any method of `Dog` that we don't like here just by reassigning it.
* It's common to set defaults in the `options` object before invoking the parent class constructor with `call`. Usually this is the only thing we do before invoking the parent class constructor.

### Extending a method

What if we want to extend what a method does, rather than completely replacing it?

Let's look at another version of `Doge`:

```javascript
function Doge(name) {
  var self = this;
  Dog.call(self, name);

  var superBark = self.bark;
  self.bark = function() {
    console.log('many');
    superBark();
  };
}

var doge = new Doge('doge');
doge.bark();
```

Here we use the *super pattern* to extend the `bark` method.

First we *capture* the old `bark` method:

```javascript
var superBark = self.bark;
```

Then we set a new one that does something extra, then invokes the original one:

```javascript
self.bark = function() {
  console.log('many');
  superBark();
};
```

*Notice that we don't have to use call or apply here, because self is lexically scoped.*

### Using methods as callbacks

Just do it:

```javascript
var doge = new Doge('doge');
setTimeout(doge.meme, 1000);
```

This wouldn't work in code that uses prototypal inheritance. We would have to write a special inline function because `this` has a different value in a timeout callback function. But with the `self` pattern it just works.

### OOP in Apostrophe 2.x

In A2 2.x, we use [moog](https://github.com/punkave/moog) for OOP in the browser, and [moog-require](https://github.com/punkave/moog-require) to provide OOP for Apostrophe modules on the server. But these are just sophisticated versions of the "self pattern." So you should definitely get familiar with the self pattern. Then check out the [moog documentation](https://github.com/punkave/moog) for more information.

It all boils down to this (on the server side):

```javascript
// in lib/modules/mymodule/index.js

module.exports = {
  beforeConstruct: function(self, options) {
    // Our chance to massage the `options` object before the base class sees it
  }
  afterConstruct: function(self) {
    // Call some methods if we need to do things right after the module is created
  },
  construct: function(self, options) {
    // Add some methods
    self.bark = function() {
      ...
    };
    self.jump = function() {
      ...
    };
  }
};
```

The browser side is very similar:

```javascript
apos.define('apostrophe-admin-bar', {

  afterConstruct: function(self) {
    self.enhance();
  },

  construct: function(self, options) {
    self.enhance = function() {
      ...
    };
  }
  
});
```

## node.js and Apostrophe (server side)

### (Almost) nothing belongs in `app.js`

Your `app.js` file should pass empty objects (`{}`) when configuring modules. The configuration for a module should live in `lib/modules/modulename/index.js`.

### Don't fight the module pattern

WRONG:

```javascript
var apos = require('apostrophe')({
  modules: {
    // THIS IS BAD
    cars: require('./lib/modules/cars/config.js')
  }
});
```

RIGHT:

```javascript
var apos = require('apostrophe')({
  modules: {
    cars: {}
  }
});
```

**Apostrophe automatically loads `lib/modules/cars/index.js`.** Put your configuration there. If you have a lot of methods, it is OK to use `require` in that file:

```javascript
// in lib/modules/cars/index.js
module.exports = {
  extend: 'apostrophe-pieces',
  name: 'car',
  someOptionName: 'someValue',
  // Lots more options here, then...
  construct: function(self, options) {
    require('./api.js')(self, options);
  }
}

// in lib/modules/cars/api.js
```
module.exports = function(self, options) {
  self.honk = function() { ... };
};
```

### Everything belongs in a module

In A2 2.x, each distinct concern should be implemented in its own module. There is an `afterInit` option available in `app.js`, but using it is a bad code smell. You should write a module instead.

If your module doesn't make sense as an extension of `apostrophe-pieces`, `apostrophe-custom-pages` or any other standard base class in Apostrophe, write a new module that doesn't extend any other:

```javascript
// lib/modules/mymodule/index.js

module.exports = {
  afterConstruct: function(self) {
    // Call some methods
  },
  construct: function(self, options) {
    // Add some methods
  } 
};
```

**All modules extend `apostrophe-module` by default**, so you can still call `render()`, etc.

You can provide an `afterInit` method in your module if you need to do stuff at startup after other modules are awake:

```
// lib/modules/mymodule/index.js

module.exports = {
  construct: function(self, options) {
    self.afterInit = function(callback) {
      // Do stuff, then invoke callback
    };
  } 
};
```

### Creating modules in A2 0.5

The [apostrophe-site](https://github.com/punkave/apostrophe-site) module README has good examples of how to create new A2 modules and subclass existing modules correctly. The boilerplate syntax is pretty annoying, so take advantage of this helpful template. In 2.x it's not such a pain.

### Always use `_` to mean "this property is temporary"

WRONG:

```javascript
// Get some events from Apostrophe, then...
_.each(events, function(event) {
  if (event happens today...) {
    event.happeningToday = true;
  }
});
```

This will clutter up the database with stale information (or worse) if the event objects get stored back to it.

So mark it as temporary:

RIGHT:

```javascript
// Get some events from Apostrophe, then...
_.each(events, function(event) {
  if (event happens today...) {
    event._happeningToday = true;
  }
});
```

Now Apostrophe knows not to save it.

**Some shops use leading `_` to mean "protected property," in the Java sense. Please don't.** We tried this and it just became confusing over time.

## Apostrophe on the browser side

### Doing things when the page loads

WRONG:

```javascript
$('.foo').on('click', function() {
  
});
```

The above code will only add an event handler to `.foo` if it is already on the page. This will fail if `.foo` is added later, for instance via the editor.

RIGHT:

```javascript
$('body').on('click', '.foo', function() {
  
});
```

The above code uses jQuery event delegation to catch all clicks on elements matching `.foo`, **even if they don't exist when this code is first called.**

ALSO RIGHT:

```javascript
apos.on('enhance', function($el) {
  $el.find('.foo').each(function() {
    var $foo = $(this);
    // Do something really fancy with $foo, like progressive enhancement
  });
});
```

Sometimes event delegation isn't enough. For those situations, use the Apostrophe `enhance` event, which fires every time Apostrophe adds new content to the page. `$el` will be the container element that has just been repopulated. 

### Tools you can count on (so don't re-install them)

The following are always available:

`async`
`lodash`
`setImmediate` (via a shim, if necessary)

And a long list of jQuery plugins:

[hotkeys](https://github.com/jeresig/jquery.hotkeys) provides portable keyboard events.
[imagesReady](https://github.com/punkave/jquery-images-ready) detects when images have loaded.
[textchange](http://zurb.com/playground/jquery-text-change-custom-event) spots changes in text fields without waiting for a blur event.
[findSafe](https://github.com/punkave/jquery-find-safe) makes it easier to implement controls that can be nested.
[onSafe](https://github.com/punkave/jquery-on-safe) plays a similar role.
[projector](https://github.com/punkave/jquery-projector) implements slideshows.
[autocomplete](https://jqueryui.com/autocomplete/) provides "typeahead" with any back end.
[draggable](https://jqueryui.com/draggable/) allows elements to be dragged.
[droppable]
sortable
alterClass
cropper
bottomless
cookie
fileupload
findByName
findSafe
getOuterHtml
jsonCall
radio
scrollintoview
selective

### Avoiding jQuery Pitfalls

#### Use `self.$el.find()`, never `$()`

When you are extending `apostrophe-modal`, **work within `self.$el`**. Don't use leaky jQuery code that accidentally targets other elements. Using `find` makes this easy.

WRONG:

```javascript
$('.my-thing').on('click', function() {
  
});
```

RIGHT:

```javascript
self.$el.find('.my-thing').on('click', function() {
  
});
```
