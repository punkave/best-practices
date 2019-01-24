# Apostrophe

## Table of Contents

1. [User Experience](#user-experience)
2. [Content options](#content-options)

## User Experience

### Locking down widgets

Often you'll have singletons for essential page components, such as navigation, hero marquees, footers, or maybe even a map on a contact page. These are things that the site admin should be able to edit, but if they're ever deleted the site or page might essentially be broken.

These elements should be locked from deletion if at all possible. There are two ways to do this.

**If you're doing these as singleton widgets**, include an options object and a `controls` object within it. On that, apply `removable: false`. Such as:

```
{{ apos.singleton(data.page, 'map', 'map', {
   controls: {
     removable: false
   }
}) }}
```

This can still be edited, but can't be removed. This means fewer panicked messages from clients when their navigation accidentally disappeared.

**If the component is truly global**, such as the navigation or footer, it's better to not do this as a widget at all.

Add fields for the components in `apostrophe-global` (ideally grouping them in the UI by component). Those can then be edited through the admin menu, and you can include the values from those fields directly in your higher-level layout templates.

## Content options

### Rich text options

ApostropheCMS enables very custom text style options in the rich text editor. This provides freedom for the project needs, but also opportunity to configure things in confusing or obfuscated ways.

We can avoid pitfalls here by thinking about a few editor-oriented principles:
- **Only include necessary styles for the specific context.** Designs often include eight or more text styles for various needs. Most of the time content editors don't need those when doing things like writing blog posts or page content. Paragraph and a few heading styles may be enough. In other, more limited contexts, such as a callout widget, fewer, or different ones, may be more appropriate.
- **Include heading levels in style labels.** Clients are increasingly concerned with SEO performance. Content is a big part of this, and we can help by making it clear what styles use what heading levels. Style labels like "Small section heading" hide this. Most of the time, labels like "Heading level 2" or "Heading 2 - Alt" (e.g., if color variants) will work.
- **Pair style and heading level appropriately.** This seems obvious, but there are times when a style using `h3` has been clearly smaller or less prominent than an `h4` style in the same editor. There might be reasons for this, but it doesn't help the content editor make good decisions.
- **Don't `div`.** There are few, if any, good reasons for a rich text editor style to use the semantically meaningless `div`. Block-level styles should use heading, paragraph, or other meaningful block level elements. Inline styles, such as to highlight words could deserve a `span`, but even then, something like [`mark`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/mark) is often better.
