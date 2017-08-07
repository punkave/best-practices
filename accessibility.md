# Accessibility

[WCAG 2.0](https://www.w3.org/TR/WCAG20/)

## Table of Contents
  1. [Language Attribute](#language-attribute)
  2. [Semantic HTML](#semantic-html)
  3. [Wayfinding](#wayfinding)
  4. [Images](#images)
  5. [ARIA](#aria)
  6. [Links](#links)
  7. [JavaScript](#javascript)
  8. [Testing](#testing)

## Language Attribute

  - Declare a language attribute on the html element so screen readers can use correct pronunciation

**[⬆ back to top](#table-of-contents)**

## Semantic HTML

### Semantic HTML Definition

  - Use semantic headings and structure to [establish a document outline](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines). Current consensus recommends using H1-6 heading rank rather than repeated H1 tags in nested sectioning elements.

### Semantic Buttons

  - Use the `button` element for JS interaction points and similar non-linking components. If a button is not of the `submit`, `reset`, or `menu` types, use `type="button"` to designate a generic button.
    - The `a` tag should only be used when using the `href` attribute to link "to other web pages, files, locations within the same page, email addresses, or any other URL" ([ref](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)).

**[⬆ back to top](#table-of-contents)**

## Wayfinding

  - Document should be navigable by keyboard

**[⬆ back to top](#table-of-contents)**

## Images

### Image HTML Tags

- Use `<img></img>` for images that are content. Use CSS background images for design "chrome".

### Image Alternative Text

  - Use alternative text for any image used as content. Alternative text should provide a description of the image. If the image is presentational or decorative use an empty `alt=""` attribute instead of `role="presentation"`.

**[⬆ back to top](#table-of-contents)**

## ARIA

  - Use ARIA Landmark roles to help assistive technologies navigate a website

**[⬆ back to top](#table-of-contents)**

## Links

### Focus State

  - Links should have a focus state `:focus`

### In Body Text

  - Links in "body" text should be underlined

### Link Purpose

  - Link purpose should be clear from the link text alone

**[⬆ back to top](#table-of-contents)**

## JavaScript

### Unobtrusive

  - Use unobtrusive JavaScript

### No-JS Alternative

  - Provide `no-js` alternatives for users that don't have JavaScript enabled

### JS Events

  - Use device agnostic events such as `onclick` and `onfocus`

### Keyboard Events

  - Provide keyboard alternative events

**[⬆ back to top](#table-of-contents)**

## Testing

  - Sites should be tested during development to ensure basic accessibility standards are being met (WCAG 2.0 AA or higher if required by the project). One simple way to do this is to install the [WAVE Toolbar browser extension](http://wave.webaim.org/extension/).

**[⬆ back to top](#table-of-contents)**
