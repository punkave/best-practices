# HTML

## Table of Contents

1. [Tables](#tables)
2. [Links](#links)

## Tables

HTML tables should be exclusively used for tabular data, such as usage reports or structured information (see example). They should not be used for layout.

Use the `scope` attribute to identify if a table header is a row or column header. Always use a descriptive `caption` and "visually hide" it if necessary for the design.

```html
<table>
  <caption>Films</caption>
  <tr>
    <th scope="col">Title/th>
    <th scope="col">Director</th>
    <th scope="col">Year</th>
  </tr>
  <tr>
    <th scope="row">Breathless</th>
    <td>Jean-Luc Godard</td>
    <td>1960</td>
  </tr>
  <tr>
    <th scope="row">The 400 Blows</th>
    <td>François Truffaut</td>
    <td>1959</td>
  </tr>
</table>
```

**[⬆ back to top](#table-of-contents)**

## Links

### Use of `target="_blank"`

#### Performance

When your page links to another page using `target="_blank"`, the new page runs on the same process as your page. If the new page is executing expensive JavaScript, your page's performance may also suffer. See a more detailed explanation [here](https://jakearchibald.com/2016/performance-benefits-of-rel-noopener/).

#### Security

When you use `target="_blank"`, the new page has access to your window object via `window.opener`, and it can navigate your page to a different URL using `window.opener.location = newURL`. Using `rel=noopener` ensures `window.opener` is `null`. See [Demo](https://mathiasbynens.github.io/rel-noopener/).
