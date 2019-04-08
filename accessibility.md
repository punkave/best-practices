# Accessibility

[WCAG 2.0](https://www.w3.org/TR/WCAG20/)

## Table of Contents
  1. [Language Attribute](#language-attribute)
  2. [Semantic HTML](#semantic-html)
  3. [ARIA](#aria)
  4. [Wayfinding](#wayfinding)
  5. [Images](#images)
  6. [Links](#links)
  7. [JavaScript](#javascript)
  8. [Email](#email)
  9. [Testing](#testing)

## Language Attribute

  - Declare a language attribute on the html element so screen readers can use correct pronunciation

**[⬆ back to top](#table-of-contents)**

## Semantic HTML

### Semantic HTML Definition

  - Use semantic headings and structure to [establish a document outline](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_HTML_sections_and_outlines). Current consensus recommends using H1-6 heading rank rather than repeated H1 tags in nested sectioning elements.

### Semantic Buttons

  - Use the `button` element for JS interaction points and similar non-linking components. If a button is not of the `submit`, `reset`, or `menu` types, use `type="button"` to designate a generic button.
    - The `a` tag should only be used when using the `href` attribute to link "to other web pages, files, locations within the same page, email addresses, or any other URL" ([ref](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a)).

### HTML5 Elements

HTML5 elements should be used whenever possible to communicate the element's role in the page. Some notable examples include:
  - [`main`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/main) for the primary content of the page, most appropriately excluding repeated elements (e.g., sidebars, decorative marquees, site header and footer)
  - [`article`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/article) for self-contained content that could conceivably belong in an RSS feed, such as blog posts, press releases, and event information (in both page and individually in index listings)
  - [`section`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/section) for thematically grouped content in a component that doesn't justify a stronger semantic element
  - [`nav`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/nav) for all significant navigation link lists
  - [`aside`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside) for complementary content, such as advertising, "related article" lists, and repeated page sidebars

Semantic HTML5 elements provide additional meaning to an HTML document for other technologies. Notably, using these roles not only serves visitors using accessibility technologies, but it also makes parsing code easier for fellow developers.

**[⬆ back to top](#table-of-contents)**

## ARIA

  - In most cases, using semantic HTML elements makes explicit designation of landmark roles unnecessary (e.g., using `role="complementary"` on an `aside` element). The exceptions would be when they are serving slightly atypical roles. An example of the latter would be setting [`role="search"`](https://w3c.github.io/aria/aria/aria.html#search) on an `aside` element when it houses a search field.
  - For more complex interactive components, use ARIA attributes to help assistive technologies navigate website functionality. Accessibility advocate Heydon Pickering has gathered [useful examples of using ARIA roles](http://heydonworks.com/practical_aria_examples/) for components such as tab groups, tooltips, and alert dialogs.

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

## Email

### Section 508

  - Satisfy each item of the HHS.gov [Email 508 Checklist](https://www.hhs.gov/web/section-508/making-files-accessible/checklist/email-508-checklist/index.html)

**[⬆ back to top](#table-of-contents)**

## Testing

  - Sites should be tested during development to ensure basic accessibility standards are being met (WCAG 2.0 AA or higher if required by the project). One simple way to do this is to install the [WAVE Toolbar browser extension](http://wave.webaim.org/extension/).

  - We currently use [pA11y](https://github.com/pa11y/pa11y-ci) for command line and CI testing of projects before merges/deployments using [apostrophe-site-map](https://github.com/apostrophecms/apostrophe-site-map) to generate a list of URLs for pA11y to crawl.

**[⬆ back to top](#table-of-contents)**
