# Apostrophe

## Table of Contents

1. [User Experience](#user-experience)

## User Experience

### Locking down widgets

Often you'll have singletons for essential page components, such as navigation, hero marquees, footers, or maybe even a map on a contact page. These are things that the site admin should be able to edit, but if they're ever deleted the site or page might essentially be broken.

These elements should be locked from deletion if at all possible. Fortunately this is pretty easy to do. In the singleton, include an options object and a `controls` object within it. On that, apply `removable: false`. Such as:

```
{{ apos.singleton(data.global, 'header', 'header', {
   controls: {
     removable: false
   }
}) }}
```

This can still be edited, but can't be removed. This means fewer panicked messages from clients when their navigation accidentally disappeared.
