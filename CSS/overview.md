# CSS

## Table of Contents
1. [Architecture](#architecture)

## Architecture
<a name="itcss"></a><a name="1.1"></a>
- [1.1](#itcss) ITCSS
Use Harry Robert's ITCSS Architecture to author CSS. ITCSS provides a way to manage common issues that crop up caused by source order, cascade and specificity in large scale projects.

ITCSS organizes CSS into eight "groups" that can even be used to organize your LESS folders if you wish. The main goal is that the compiled CSS file is structured (top to bottom) in order of specificity.

- **Settings** - Variables
- **Tools** - Mixins
- **Generic** - Ground zero styles (normalize.css, resets)
- **Base** - Unclassed HTML elements
- **Objects** - UI Abstractions (object-oriented css)
- **Components** - One off designed components (footer, header, etc.)
- **Utilities** - Single purpose (immutable) selectors

It's also a helpful pattern to prefix selectors (i.e. `.o-, .c-, .u-`) using these group names so that it's easy to identify the role of a selector in markup.

<a name="oocss"></a><a name="1.2"></a>
- [1.1](#itcss) OOCSS
Author CSS with the [two main principles](https://github.com/stubbornella/oocss/wiki#two-main-principles-of-oocss) of object-oriented css in mind. Rarely use location dependent styles. This ensures that selectors can be reused anywhere and are context agnostic.

### Bad
```less
// Using cascade
.context {
  .aside {
    padding-bottom: 3rem;

    .title {
      padding-bottom: 4rem;
    }
  }
}
```

### Good
```less
// OOCSS
.context { ... }
.aside { ... }
.aside--context { padding-bottom: 3rem; }
.title { ... }
.title--context { padding-bottom: 4rem; }
```
