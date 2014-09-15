# Ordering CSS Rules
We are by no means attempting to innovate here
because we've found that someone has already done a fairly good job of describing a usable, predictable pattern to ordering CSS rules.
Namely, we've prescribed to the method that [@mdo](http://github.com/mdo) set forth in his [Code Guide](http://codeguide.co/#css-declaration-order)

## P.B.T.V. or Peanut Butter TeleVision
The ideal order of CSS rules within a single selector's scope is as follows: `positioning`, `box-model`, `typography`, and `visual`.
This order can be followed by a miscellaneous category that allows a developer to place hard-to-categroize rules simply at the end.
This of course bears an example:

```css
/* Pattern */
/*-------------------------------------------*/
.selector
{
  /* Start with Positioning */
  position: absolute;
  top: 0;
  right: 0;
  bottom: 0;
  left: 0;
  z-index: 1;

  /* Then the Box Model */
  display: block;
  float: left;
  clear: both;
  width: 20rem;
  height: 20rem;
  margin: 0 0 0 0;
  padding: 0 0 0 0;

  /* Then Typography */
  font-family: 'Helvetica', sans-serif;
  font-size: 2rem;
  line-height: 1.4;
  font-weight: bold;
  font-style: italic;
  color: #333;

  /* Then Visual */
  background-color: #eee;
  border: 1px solid #999;

  /* And Miscellaneous */
  transform: translateX(90);

```

## Further Reading
* ["Code Guide" -Mark Otto, @mdo](http://codeguide.co/#css-declaration-order)
* ["Wordpress Coding Standards for CSS"](http://make.wordpress.org/core/handbook/coding-standards/css/#property-ordering)
  * *N.B. Although P'unk Ave is not a Wordpress shop, the Wordpress standards take a similar approach to @mdo's Code Guide in terms of property ordering.*


[Back to All CSS Articles](/CSS/overview.md)
