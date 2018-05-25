# Apostrophe

## Table of Contents

1. [User Experience](#user-experience)

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
