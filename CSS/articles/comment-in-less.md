# Comments in LESS
Commenting is one of those things that we shouldn't have to think about too much when writing styles. They're important for support that inevitably comes later on a web project, but they're very easy to forget. So we came up with a few basic rules and accompanying snippets for both SublimeText and Atom.

## File Headers
At the top of each file, add a simple title comment letting other developers know what the general scope of the file is. To add a SublimeText snippet, follow the instructions [here](http://sublimetext.info/docs/en/extensibility/snippets.html), and to add an Atom Snippet, follow the instructions [here](https://github.com/atom/snippets).
```cson
# SublimeText Snippet
<snippet>
  <content><![CDATA[
// ===============================================================
// $1
// ===============================================================
]]></content>
  <tabTrigger>line</tabTrigger>
  <scope>source.less</scope>
</snippet>

# Atom Snippet
'.source.less':
  'Long Comment Line':
    'prefix': 'line'
    'body': """
            // ===============================================================
            \// $1
            \// ===============================================================
            """
```

## In-file Subtitles
Occasionally (and it should only be occasionally), a LESS file can grow beyond a few simple rules. In those cases, we need to break up the file using section headers to help other developers make sense of your structural thinking.
```cson
# SublimeText Snippet
<snippet>
  <content><![CDATA[
// $1 ===================================
]]></content>
  <tabTrigger>title</tabTrigger>
  <scope>source.less</scope>
</snippet>

# Atom Snippet --> If you've add the previous snippet, you don't need to have the first line with '.source.less'
'.source.less':
  'Title Comment Line':
    'prefix': 'title'
    'body': """
            // $1 ===================================
            """
```
[Back to All CSS Articles](/CSS/overview.md)
