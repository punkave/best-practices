# HTML

## Table of Contents

1. [Tables](#tables)

## Tables

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
