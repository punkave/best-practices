# CSS

Use [stylelint](https://stylelint.io/) and the P'unk Ave [stylelint configuration](https://www.npmjs.com/package/stylelint-config-punkave) to lint CSS. There's an [Atom plugin](https://atom.io/packages/linter-stylelint) available for stylelint. If you don't have stylelint validating with a build tool or test you need to rely on your editor for linting.

## Table of Contents
1. [Architecture](#architecture)
2. [Naming](#naming)
3. [Sizing](#sizing)
4. [Stacking Order](#stacking-order)
4. [Preprocessor Practices](#preprocessor-practices)

## Architecture

### ITCSS
Use Harry Robert's [ITCSS Architecture](https://www.youtube.com/watch?v=1OKZOV-iLj4) to author CSS. ITCSS provides a way to manage common issues that crop up caused by source order, cascade and specificity in large scale projects.

ITCSS organizes CSS into eight "groups" that can even be used to organize your LESS folders if you wish. The main goal is that the compiled CSS file is structured (top to bottom) in order of specificity.

- **Settings** - Variables
- **Tools** - Mixins, Functions
- **Generic** - Ground zero styles (normalize.css, resets)
- **Base** - Unclassed HTML elements
- **Objects** - UI Abstractions (object-oriented css)
- **Components** - One off designed components (footer, header, etc.)
- **Utilities** - Single purpose (immutable) selectors

It's also a helpful pattern to prefix selectors (i.e. `.o-, .c-, .u-`) using these group names so that it's easy to identify the role of a selector in markup.

### Location Dependency
Only use location dependent styles rarely.

> Why? This ensures that selectors can be reused anywhere and are context agnostic.

```less
// Good
.context {
  border: 1rem solid #ccc;
}
.aside--context {
  padding-bottom: 3rem;
}
// Bad
.context {
  border: 1rem solid #ccc;
  .aside {
    padding-bottom: 3rem;
  }
}
```

**[⬆ back to top](#table-of-contents)**

## Naming

### Selectors
Name selectors by role in UI instead of semantically.

> Why? This decouples rigid, visual semantics from the UI

```less
// Good
.title-primary {
  font-family: 'Helvetica';
  font-size: 2rem;
  color: black;
}
// Bad
.title-black {
  font-family: 'Helvetica';
  font-size: 2rem;
  color: black;
}
```

### Variables
Use abstractions to name variables.

> Why? This makes the UI flexible to visual change.

```less
// Good
@border-primary: 1rem solid green;
// Bad
@green-border: 1rem solid green;
```

**[⬆  back to top](#table-of-contents)**

## Sizing

### Sizing Units

- Use `px` for borders
- Use unitless line height
- Use `em` for media queries
- Use `rem` for everything else (font size, spacing, etc.)
- `%` is also appropriate and can be used for column widths, etc.

### Typography

> Why? A relative, root font size is important so that type scales appropriately if a user has their type size settings adjusted in their browser.

Set a root `font-size` of `62.5%` on the `html` selector, which is roughly equivalent to `10px` (an authoring convenience, `1.6rem` is equal to `16px`) across browsers.

**[⬆  back to top](#table-of-contents)**

## Stacking Order

### Z-index
Use variables or maps to manage z-index values. Variables or keys should be named semantically, such as "z-index-modal". Values should ideally be set `1, 2, 3, 4` etc. and shouldn't be set arbitrarily. Declarations like `z-index: 9999;` should never be used and always avoid `!important`. Store all variables or maps in a partial or single place.

**[⬆  back to top](#table-of-contents)**

## Preprocessor Practices

### Sass extends

Using the `@extend` directive in Sass is a good tool to keep code DRY, but it [can also lead to unintentional exponential bloat](http://thesassway.com/intermediate/understanding-placeholder-selectors). To prevent this, **the `@extend` directive should only be used with placeholder selectors** (e.g., `%class-name`).

The short version of the reasoning is that the placeholder can be limited to precisely what should be duplicated. Extending an actual class can unintentionally duplicate any rules where that class is present, even if not desired. [More here](http://alexbea.com/2016/01/10/sass-extends.html) for the extended reasoning.

For example:
```css
%btn-style {
  // CSS declarations
}

.o-button {
  @extend %btn-style;
}

.o-tab {
  @extend %btn-style;
}
```

Compiles to:
```css
.o-button,
.o-tab {
  // CSS declarations
}

// @NOTE: Placeholder class is not compiled.
```

**[⬆  back to top](#table-of-contents)**
