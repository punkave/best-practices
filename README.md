:pray::pray::pray:

## How to add to this repo

* Discuss topic with group during regular developer meeting or open a GitHub issue to begin discussion
* If discussed during the meeting, someone on the team should capture notes in a GitHub issue discussion and supply a few code examples for further "offline" discussion
* The developer responsible for the topic creates a pull request to this repo with changes or additions reflecting the consensus of the group

## Our Guiding Principles

We use these as a lens with which to examine potential best practices:

* **Elegance** - We design and build beautifully elegant applications and websites, this beauty should extend to the codebase as well.
* **Speed** - We move quickly. We hit our milestones. We ship.
* **Pragmatism** - We are pragmatic in our solutions. We don't over-engineer things. No reinventing the wheel. No [yak shaving](https://en.wiktionary.org/wiki/yak_shaving).
* **Maintainability** - We maintain relationships with projects over years. This means we write maintainable code: well organized, well documented, well supported.
* **Innovation** - We look for opportunities to experiment with emerging technologies and techniques. We learn.

Also see the [Pragmatic Programmer Quick Reference Guide](http://www.ccs.neu.edu/home/lieber/courses/csg110/sp08/Pragmatic%20Quick%20Reference.htm) for additional rules to code by.

# Table of Contents

_Instead of having our content buried in folders, I'm proposing we pull the majority of it out onto this main README page. Below, I have built out a table of contents suggesting the topics we should cover with a few brief sentences and some code examples. Under some topics/sections, I started to bullet out points we might want to cover._

_I kept JavaScript in a separate file so far because it is extensive. -Tom_

#### [House Style Rules](#house)
* [CSS Architecture](#house--css)
* [Javascript](javascript/overview.md)
* [Markup](#house--markup)
* [Sizes](#house--sizes)
* [Naming less variables](#house--naming)
* [Our Grid](#house--grid)

#### [Setting up a new project](#newProject)
* [Choosing the shortname (and everything-name) of your project](#newProject--shortname)
* [Forking a project to start a "2.0 version" while 1.0 or the "one-pager" lives on](#newProject--2)
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

#### [Production](#production)
* [Overview and tools used](#production)
* [Production DNS](#production--dns)
* [Production Platform](#production--dns)
* [Production Multicore](#production--multicore)

# <a name="production">Production overview and tools used</a>

**The support team is normally responsible for the initial launch of websites,** including provisioning of production servers. We always offer managed hosting services to the client.

We use [mechanic](https://npmjs.org/package/mechanic) and [Stagecoach](https://github.com/punkave/stagecoach).

`mechanic` manages nginx, which we use as a reverse proxy, so that Node.js is not responsible for static files, load balancing, HTTPS or starting as root to capture port 80. 

We use `stagecoach` to handle deployments in a way that minimizes downtime, rules out any issues with cached assets and permits rollback.

## <a name="production--dns">Production DNS</a>

We always add a DNS entry that looks exactly like this:

`MYSHORTNAME-prod.punkave.net`

So that the server can be immediately referred to before production DNS changes are made. We continue to use this name when configuring backups, `deployment/settings.production` files, etc. so that we do not confuse caching CDN services like CloudFlare with our actual production server.

We ask our clients to make their official DNS changes for production launches via the control panel of their domain registrar. Consolidating DNS and domain registration with one company reduces the number of moving parts for them.

Our Stagecoach recipes use `forever` to keep Node processes running.

## <a name="production--platform">Production Platform</a>

Our current recommended platform is CentOS 7, with EPEL 7. EPEL 7 provides Node.js 6.x and MongoDB 2.6.x and will hopefully do so until 2023.

## <a name="production--multicore">Multicore / multi-process</a>

We always run at least two Node.js server processes per production site to provide levels of reliability akin to that which comes standard with "all in one" solutions like Apache's PHP module. See the [multicore HOW-TO](http://apostrophecms.org/docs/tutorials/howtos/multicore.html).

#### Special topics
_Here, we would link into specific articles with more in-depth information_

* [Performance in Apostrophe](#)
* [Accesibility](#)
* [Importing spreadsheet data into apostrophe-pieces based content types](#)
* [Workflow for frontend-only / static site builds](#)
* [What to do when your client needs a placeholder site before their full-featured a2 build](#)

# <a name="house">House Style Rules</a>

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

## <a name="newProject-shortname">Choosing the shortname (and everything-name) of your project</a>

The "shortname" of your project, like "ccd" or "pogil" or "thenotebook", must also be the name of the repo, the name of the database, the name of the directory on the server (and on your hard drive), and the name of everything else that requires a unique name for the project. Do everything you can to avoid diverging on this point.

The shortname may contain lowercase letters, digits, and hyphens. Nothing else.

The shortname should be short, but consistency is even more important than brevity.

## <a name="newProject-2">Forking a project to start a "2.0 version" while "1.0" lives on</a>

**If a project is "forked," for instance to create a full site from a placeholder site, and the placeholder site continues to be live in the meantime, the shortname / database name / everything name must be different for the fork.** This avoids ugliness when we inevitably want to deploy the new site side by side with the old during a transitional period as new content is created.

A common example is a case that begins life as a "one-pager" and then goes into development as a larger site. They must coexist temporarily and the customer will want to use the same server to manage costs.

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
