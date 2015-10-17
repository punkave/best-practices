# JavaScript Style Guide

JavaScript at P'unk Avenue follows a consistent style. What follows is a summary of that style. These are practices that are always followed in our npm modules and should ideally be followed in project-level and other non-Apostrophe code as well.

Let's start with the basics.

## Indentation

Two spaces. Always. Not tabs. Set up your editor accordingly and it'll automatically do this for you.

## Always use curly braces

WRONG:

```javascript
if (test()) doTheThing();
```

On one line, it's harder to read unless it's trivial. If you break it over multiple lines, and easier to wind up with bugs when you accidentally try to introduce a second statement. Apple got stuck with a famous security bug this way. Just don't use it.

RIGHT:

```javascript
if (test()) {
  doTheThing();
}
```

The opening curly brace appears on the *same line* with `if`, `while`, etc. The closing curly brace appears on a line of its own.

## Always declare variables at the start of the function

Declaring a variable with `var` inside a "while" or "for" loop DOES NOT make a unique variable for every pass through the loop. And that will mess you up good when you start trying to access them in [closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures).

To avoid confusion, always put your `var` statements *at the start of a function*.

Declaring variables in nested functions is perfectly OK.

## Always declare variables!

Otherwise they are global, resulting in strange bugs and side effects.

Ideally you should use [JSHint](http://jshint.com/) so that your editor tells you anytime you forget to use `var`.

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

"Why?" Because every time you call a function, JavaScript has to remember where you called from on a "stack." And if you never return, but keep calling deeper and deeper, the "stack" eventually crashes. For instance, the first version of `handleTheThing` will probalby crash if you pass it to `async.eachSeries` with an array of 2000 things to process.

### Always use the async module

The async module solves all the terrible problems you'll encounter with callback-based functions. Learn it, love it, use it.

#### async.series

When you're working with functions that take callbacks, but you want things to happen in a certain order, it's easy to write confusing code. The async.series method exists to prevent this problem.

*The async module is always available in browser-side JavaScript in Apostrophe. In node you require it.*

Let's say we want to do three things in a sequence.

WRONG:

```javascript
doSomethingThen(function(err) {
  // handle err, then...
  doSomethingElseThen(function(err) {
    // handle err, then...
    doYetAnotherThingThen(function(err) {
      // handle err, then...
    })
  })
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

"What about `async.parallel`?" Keep it simple and run things in series. Unless you know for a fact that your functions never, ever depend on each other's work and your database won't mind all the extra simultaneous queries, times as many people as are looking at the website, there's really no reason to go there.

#### async.eachSeries

What if you have an array of objects that all need to be processed by a function that takes a callback? Process them one at a time with `async.eachSeries`, so that you know when your code has completed, and you don't overwhelm your database qith queries.

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

Here we're processing up to 4 things at a time. A good technique if you know you have, let's say, enough RAM to process four image imports at once.

## Using ES6 features

Don't, yet. Unless you are pre-compiling them to ES5 first with a tool like Babel. Our clients need to support a variety of browsers and browser versions.

*ES6 is a recently proposed version of JavaScript.*

## Object-oriented programming

In A2 0.6, we use [moog](https://github.com/punkave/moog) for OOP in the browser, and [moog-require](https://github.com/punkave/moog-require) to provide OOP for "Apostrophe modules" on the server. But these are just more sophisticated versions of the "self pattern" explained below, so you should definitely read on before reading up on them.

### The "self pattern" at P'unk Avenue

There are many object-oriented programming styles in JavaScript, because the language provides a set of tinkertoys to create them. Most of them do depend on the `this` keyword, which is basically broken for asynchronous programming. Our house style avoids this problem at a very small cost in performance. We've never seen that matter in practice, even with tens of thousands of objects in play.

It's often described as the "self pattern." It's also a form of "concatenative inheritance.""

Here's a simple "class" following our style:

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

* The methods of the object live inside the constructor function, `Dog`.

* The constructor takes one argument, `options`, which is an object. This makes it easier to extend the code without bc breaks (*backwards compatibility*).

* The `this` keyword is used exactly once, to capture the object in `self`, a lexically scoped variable that always refers to the same things, *even in a callback function or jQuery event handler*. And we never, ever use it again.

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

This wouldn't work in code that uses prototypal inheritance. We would have to write a special inline function because `this` has a different value in a timeout callback function. With the `self` pattern it just works.

between objects.

### 0.6 note

In A2 0.6, we use [moog](https://github.com/punkave/moog) for OOP in the browser, and [moog-require](https://github.com/punkave/moog-require) to provide OOP for "Apostrophe modules" on the server. But these are just sophisticated versions of the "self pattern." So you should definitely get familiar with the above. Then check out the [moog documentation](https://github.com/punkave/moog) for more information.

## node.js and Apostrophe (server side)

### Always be requiring

WRONG (this is an A2 0.5 example):

```javascript
var site = require('apostrophe-site')();

site.init({
  // Among other things...
  pages: {
    load: [
      function(req, callback) {
        // 200 lines of guff here
      },
      function(req, callback) {
        // 200 more lines..
      },
    ]
  }
});
```

RIGHT:

```javascript
var site = require('apostrophe-site')();

site.init({
  // Among other things...

  pages: {
    load: [
      require('./lib/loaders/myCatLoader.js')(site),
      require('./lib/loaders/myDogLoader.js')(site)
    ]
  }
});

// In the SEPARATE FILE lib/loaders/myCatLoader.js

module.exports = function(site) {
  return function(req, callback) {
    // 200 lines...
    // You can use site.apos, etc.
  });
};
```

The same principle applies whether you're writing an Apostrophe site or not: use `require` to break up your code into units with meaningful names.

### Creating modules in A2 0.5

The [apostrophe-site](https://github.com/punkave/apostrophe-site) module README has good examples of how to create new A2 modules and subclass existing modules correctly. The boilerplate syntax is pretty annoying, so take advantage of this helpful template. In 0.6 it's not such a pain.

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

This will clutter up the database if the event objects happen to get stored back to it.

Mark it as temporary:

RIGHT:

```javascript
// Get some events from Apostrophe, then...
_.each(events, function(event) {
  if (event happens today...) {
    event._happeningToday = true;
  }
});
```

Now `apos.putPage` will know not to save it. *This is still true in A2 0.6 even though the method names have changed.*

Some shops use leading `_` to mean "protected property," in the Java sense. Please don't. We tried this and it just became confusing over time.

## Apostrophe on the browser side

### Doing things when the page loads

```javascript
apos.on('ready', function() {
  // Play with jQuery here
});
```

This is the right way to make sure your code runs every time the main content area of the page is refreshed. If you use `$(function() { ... })`, your code only runs on the first page load. Which leads to "you have to click refresh after you click save" syndrome. Which makes puppies cry. So do the right thing.

