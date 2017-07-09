# Accessibility

[WCAG 2.0](https://www.w3.org/TR/WCAG20/)

## Table of Contents
  1. [Language Attribute](#language-attribute)
  2. [Document Outline](#document-outline)
  3. [Wayfinding](#wayfinding)
  4. [Images](#images)
  5. [ARIA](#aria)
  6. [Links](#links)
  7. [JavsScript](#javascript)

## Language Attribute

  <a name="languageAttribute--definition"></a><a name="1.1"></a>
  - [1.1](#languageAttribute--definition) Declare a language attribute on the html element so screen readers can use correct pronunciation

**[⬆ back to top](#table-of-contents)**

## Semantic HTML

  <a name="semanticHtml--definition"></a><a name="2.1"></a>
  - [2.1](#semanticHtml--definition) Use semantic headings and structure to [establish a document outline](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines). Current consensus recommends using H1-6 heading rank rather than repeated H1 tags in nested sectioning elements.
  <a name="semanticHtml--buttons"></a><a name="2.1"></a>
  - [2.2](#semanticHtml--buttons) Use the `button` element for clickable JS interaction points and similar non-linking components. If not of the `submit`, `reset`, or `menu` types, use `type="button"` to designate a generic button.
    - The `a` tag should only be used when using the `href` attribute to link "to other web pages, files, locations within the same page, email addresses, or any other URL" ([ref](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)).

**[⬆ back to top](#table-of-contents)**

## Wayfinding

  <a name="wayfinding--definition"></a><a name="3.1"></a>
  - [3.1](#wayfinding--definition) Document should be navigable by keyboard

**[⬆ back to top](#table-of-contents)**

## Images

  <a name="images--html"></a><a name="4.1"></a>
  - [4.1](#images--html) Use `<img></img>` for images that are content. Use CSS background images for design "chrome".

  <a name="images--alternatives"></a><a name="4.2"></a>
  - [4.2](#images--alternatives) Use alternative text for any image used as content. Alternative text should provide a description of the image.

**[⬆ back to top](#table-of-contents)**

## ARIA

  <a name="aria--definition"></a><a name="5.1"></a>
  - [5.1](#aria--definition) - Use ARIA Landmark roles to help assistive technologies navigate a website

**[⬆ back to top](#table-of-contents)**

## Links

  <a name="links--focus-state"></a><a name="6.1"></a>
  - [6.1](#links--focus-state) Links should have a focus state `:focus`

  <a name="links--in-body"></a><a name="6.2"></a>
  - [6.2](#links--in-body) Links in "body" text should be underlined

  <a name="links--purpose"></a><a name="6.3"></a>
  - [6.3](#links--purpose) Link purpose should be clear from the link text alone

**[⬆ back to top](#table-of-contents)**

## JavaScript

  <a name="javascript--unobtrusive"></a><a name="7.1"></a>
  - [7.1](#javascript--unobtrusive) Use unobtrusive JavaScript

  <a name="javascript--alternative"></a><a name="7.2"></a>
  - [7.2](#javascript--alternative) Provide `no-js` alternatives for users that don't have JavaScript enabled

  <a name="javascript--events"></a><a name="7.3"></a>
  - [7.3](#javascript--events) Use device agnostic events such as `onclick` and `onfocus`

  <a name="javascript--keyboard-events"></a><a name="7.4"></a>
  - [7.4](#javascript--keyboard-events) Provide keyboard alternative events

  **[⬆ back to top](#table-of-contents)**
