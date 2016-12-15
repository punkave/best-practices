:pray::pray::pray:

## How to add to this repo

* Discuss topic with group duing Thursday morning meeting
* Someone on the team should capture notes in a Github issue discussion and supply a few code examples for further discussion
* Create a pull request to this repo with changes or additions reflecting the consensus of the group

## Our Guiding Pricipals

We use these as a lense with which to examine potential best practices:

* 1.
* 2.
* 3.
* 4.
* 5.

# Table of Contents

_Instead of having our content buried in folders, I'm proposing we pull the majority of it out onto this main README page. Below, I have built out a table of contents suggesting the topics we should cover with a few breif sentences and some code examples. Under some topics/sections, I started to bullet out points we might want to cover._

#### [House Style Rules](#house)
* [CSS Architecture](#house--css)
* [Javascript](#house--javascript)
* [Markup](#house--markup)
* [Sizes](#house--sizes)
* [Naming less variables](#house--naming)
* [Our Grid](#house--grid)

#### [Setting up a new project](#newProject)
* [Using Apostrophe CLI / client boilerplate](#newProject--cli)
* [Importing the final launch checklist](#newProject--checklist)
* [Setting up package.json](#newProject--package)
* [Setting up app.js](#newProject--app)
* [Project folder structure](#newProject--folders)
* [Naming modules](#newProject--naming)

#### [Front-end Styleguides](#styleguides)

#### [Responsive](#responsive)
* [Mobile](#responsive--mobile)
* [Cross Browser](#responsive--xbrowser)
* [Accessibility](#responsive--accesibility)

#### Special topics
_Here, we would link into specific articles with more in-depth information_

* [Performance in Apostrophe](#)
* [Accesibility](#)
* [Importing spreadsheet data into apostrophe-pieces based content types](#)
* [Workflow for frontend-only / static site builds](#)
* [What to do when your client needs a placeholder site before their full-featured a2 build](#)


# <a name="house">House Style Rules

## <a name="house--css">CSS Architecture</a>

Use Harry Robert's ITCSS Architecture to author CSS. ITCSS provides a way to manage common issues that crop up caused by source order, cascade and specificity in large scale projects.

ITCSS organizes CSS into eight "groups" that can even used to organize your LESS folders if you wish. The main goal is that the compiled CSS file is structured (top to bottom) in order of specificity.

- **Settings** - Variables
- **Tools** - Mixins
- **Generic** - Ground zero styles (normalize.css, resets)
- **Base** - Unclassed HTML elements
- **Objects** - UI Abstractions (object-oriented css)
- **Components** - One off designed components (footer, header, etc.)
- **Utilities** - Single purpose (immutable) selectors

It's also helpful pattern is to prefix selectors (i.e. `.o-, .c-, .u-`) using these group names so that it's easy to identify the role of a selector in markup.

## <a name="house--javascript">Javascript</a>
  * nest things in apos events where applicable
  * never attach anything to the global namespace
  * use data attributes for js selectors
  * use .find to keep your jquery scopes tight
  * be good at async
  
## <a name="house--markup">Markup</a>
  * html 5 elements like header, footer, etc.
  * when to use divs, spans, uls, you should know this
  
## <a name="house--sizes">Sizes</a>
  * rems, ems, px, %, when to use what
  
## <a name="house--naming">Naming less variables</a>
  * general to specific
  * use names like `brand-primary` and `brand-secondary` for colors
  
## <a name="house--grid">Our Grid</a>

# <a name="newProject">Setting up a new project</a>

## <a name="newProject--cli">Using Apostrophe CLI / client boilerplate</a>

## <a name="newProject--checklist">Importing the final launch checklist</a>

## <a name="newProject--package">Setting up package.json</a>
  * including dependencies
  * configuring start script `npm start` vs `node app`
  
## <a name="newProject--app">Setting up app.js</a>
  * shortnames
  * security
  * other config stuff
  
## <a name="newProject--folders">Project folder structure</a>
  * components / macros
  * pages
  * less
  * client-side js
  
## <a name="newProject--naming">Naming modules</a>

# <a name="styleguides">Front-end Styleguides</a>
???

# <a name="responsive">Responsive</a>
### <a name="responsive--mobile">Mobile</a>
  * The bare minimums
  * naming your breakpoints
  * where to put responsive styles

### <a name="responsive--xbrowser">Cross Browser</a>
  * The bare minimums
  * Tools we use for testing

### <a name="responsive--accessibility">Accessibility</a>
  * The bare minimums
  * Docs for WCAG etc.
