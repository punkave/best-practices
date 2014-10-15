# Following a JS Style Guide

Rather than writing our own Javascript style guide we can refer to [Airbnb's guide](https://github.com/airbnb/javascript), which looks a lot like the code we've been writing.

One notable exception to this style guide: we use the variable name `self` to cache the reference of a specific context, whereas the Airbnb guide uses `_this`. You can read more about the self pattern and why we use it in Tom's article ["This" Considered Harmful (Sometimes)](http://justjs.com/posts/this-considered-harmful).