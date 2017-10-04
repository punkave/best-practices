# General Practices

## Table of Contents

1. [Code Commenting](#code-commenting)

## Code Commenting

### Linter disabling reasons

Our linter rules, with ESLint and Stylelint, can be disabled for a file, for a code block, or for a specific line when needed. See documentation [for Stylelint](https://github.com/stylelint/stylelint/blob/master/docs/user-guide/configuration.md#turning-rules-off-from-within-your-css) and [for ESLint](https://eslint.org/docs/user-guide/configuring#disabling-rules-with-inline-comments) regarding their methods.

If our linter rules are well thought-out (and open to suggestions from the team), this should be done very rarely. When disabling a linter rule, **we should include an additional comment line briefly noting why it was done**. This helps code reviewers in their job and helps devs later maintain the code by indicating why the irregular decision was made.

For example:
```css
body {
  // stylelint-disable-next-line property-no-vendor-prefix
  -webkit-text-size-adjust: 100%;
  // Prefix property needed for iOS font sizing.
}
```
