# Naming CSS Classes
This is a really complex issue that we're still in the midst of working out.
For the time being, we have landed on a modular, visually-semantic, and extensive protocol for naming our CSS classes.

## Modular
By modular, we mean that as much as possible, CSS classes should be broad enough to be re-used in other cases.
This has a few practical applications:
#### 1. Classes should not be tied to elements so that they can be used in any markup.
For example, a set of styles that controls a list-like display for elements might have broader application than either a `ul` or `ol` tag.
```css
/* Antipattern */
ul.list-items
{
  display: block;
  padding: 10px 0;
}

/* Pattern */
.list-items
{
  display: block;
  padding: 10px 0;
}
```

#### 2. The class name must itself be just broad-enough to invite modular reuse.
Using our list-like item from the previous example, we ought to create a class name that is neither too broad (i.e. I can't tell what it applies to) nor too specific (I'm afraid to apply or modify it).
```css
/* Antipattern -- Too Specific */
.blog-sidebar-list-items
{
  display: block;
  padding: 10px 0;
}

/* Antipattern -- Too Broad */
.items
{
  display: block;
  padding: 10px 0;
}

/* Pattern */
.list-items
{
  display: block;
  padding: 10px 0;
}
```
## Visually Semantic
By visually semantic, we mean that class names ought to point to an easily recognizable visual element from the original design or final presentation.
There's a tendency to have class names reflect CSS rules in an object-oriented kind of way (".display-inline-block" or ".background-blue"),
but we think this tends away from modularity and so using something more visually semantic can help us abstract more easily and not always tie classes to a single rule or function.
For instance, let's take an example where an element (here a button) is designed to overlap another element:  
```css
/* Antipattern -- Too Rule Based */
.negative-margin-top
{
  /* Unlikely to include other styles */
  margin-top: -10px;
}

/* Antipattern -- Too Abstracted */
.up
{
  /* Could include other styles, but may not be recognizable */
  margin-top: -10px;
}

/* Pattern */
.overlap
{
  /* Could include other styles about our component */
  margin-top: -10px;
}
```

## Extensive
Component driven class names allow us to create standard, normal cases that should cover most instances of a component.
However, deviations from that norm occur, and our naming convention needs to take this into account.
In these cases, we prefer a double-dashed extending name.
In our example below, we take a common case of a button which needs to have a different background color:
```css
/* Antipattern -- Not distinguished */
.button
{
  /* All the buttons styles */
}
.button-orange
{
  background-color: orange;
}

/* Pattern */
.button
{
  /* All the buttons styles */
}
.button--orange
{
  background-color: orange;
}

```
There are some specific LESS anti-patterns to avoid here as well.
One approach to this extensive pattern is to store the values for our `.button` class in a LESS mixin.
The perceived advantage here is the use of one class in the markup (`<a href="#" class="button-orange">` vs `<a href="#" class="button button--orange">`).
```css
/* Antipattern -- Storing normalized component in mixin */
.button
{
  display: inline-block;
  margin: 0 5px;
  padding: 5px 15px;
  border: 1px solid #fff;
}
.button-orange
{
  .button();
  background-color: orange;
}
.button-blue
{
  .button();
  background-color: blue;
}
/* Pattern */
.button
{
  display: inline-block;
  margin: 0 5px;
  padding: 5px 15px;
  border: 1px solid #fff;
}
.button--orange
{

  background-color: orange;
}
.button--blue
{
  .button();
  background-color: blue;
}
```
The problem here lies in the finally rendered style sheet. Here are the compiled results:
```css
/* Antipattern Results */
.button-orange
{
  display: inline-block;
  margin: 0 5px;
  padding: 5px 15px;
  border: 1px solid #fff;
  background-color: orange;
}
.button-blue
{
  display: inline-block;
  margin: 0 5px;
  padding: 5px 15px;
  border: 1px solid #fff;
  background-color: blue;
}

/* Pattern Results */
.button
{
  display: inline-block;
  margin: 0 5px;
  padding: 5px 15px;
  border: 1px solid #fff;
}
.button--orange
{
  background-color: orange;
}
.button--blue
{
  background-color: blue;
}
```
In our case, we may have only saved two lines of code, but as the exceptions grow, the size of our compiled CSS can grow substantially.
Therefore, it's our preference to go with a second, extensive class that covers the deviations from a component's base class.

## Further Reading
* ["About HTML semantics and front-end architecture" -Nicolas Gallagher](http://nicolasgallagher.com/about-html-semantics-front-end-architecture/)
* ["Naming CSS Stuff is Really Hard" -Ethan Muller](http://seesparkbox.com/foundry/naming_css_stuff_is_really_hard)

[Back to All CSS Articles](#)
