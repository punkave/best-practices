:pray::pray::pray:

## How to add to this repo

* Discuss topic with group during regular developer meeting or open a GitHub issue to begin discussion
* If discussed during the meeting, someone on the team should capture notes in a GitHub issue discussion and supply a few code examples for further "offline" discussion
* The developer responsible for the topic creates a pull request to this repo with changes or additions reflecting the consensus of the group

## Our Guiding Pricipals

We use these as a lens with which to examine potential best practices:

* **Elegance** - We design and build beautifully elegant applications and websites, this beauty should extend to the codebase as well.
* **Speed** - We move quickly. We hit our milestones. We ship.
* **Pragmatism** - We are pragmatic in our solutions. We don't over-engineer things. No reinventing the wheel. No [yak shaving](https://en.wiktionary.org/wiki/yak_shaving).
* **Maintainability** - We maintain relationships with projects over years. This means we write maintainable code: well organized, well documented, well supported.
* **Innovation** - We look for opportunities to experiment with emerging technologies and techniques. We learn.

Also see the [Pragmatic Programmer Quick Reference Guide](http://www.ccs.neu.edu/home/lieber/courses/csg110/sp08/Pragmatic%20Quick%20Reference.htm) for additional rules to code by.

# Table of Contents

_Instead of having our content buried in folders, I'm proposing we pull the majority of it out onto this main README page. Below, I have built out a table of contents suggesting the topics we should cover with a few breif sentences and some code examples. Under some topics/sections, I started to bullet out points we might want to cover._

#### [House Style Rules](#house)
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

#### [Collaborative Coding](#collaboration)
* [Git Workflow](#collaboration--git)
* [Branch Naming](#collaboration--git-naming)

#### [Travis CI](#travis)
* [Getting Started](#travis--getting-started)

#### Special topics
_Here, we would link into specific articles with more in-depth information_

* [Performance in Apostrophe](#)
* [Accesibility](#)
* [Importing spreadsheet data into apostrophe-pieces based content types](#)
* [Workflow for frontend-only / static site builds](#)
* [What to do when your client needs a placeholder site before their full-featured a2 build](#)

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

# <a name="collaboration">Collaborative Coding</a>
### <a name="collaboration--git">Git Workflow</a>

  TODO: Locking down master


  Some basic rules to follow when developing on teams:

  * Anything in the master branch is deployable
  * To work on something new, create a descriptively named branch off of master (ie: `feature/new-oauth2-scopes`)
  * Commit to that branch locally and regularly push your work to the same named branch on the server
  * When you need feedback or help, or you think the branch is ready for merging, open a pull request
  * After someone else has reviewed and signed off on the feature, you can merge it into master
  * Once it is merged and pushed to `master`, you can and should deploy immediately


  The above list is from the article [GitHub Flow](http://scottchacon.com/2011/08/31/github-flow.html) by Scott Chacon

  To get a new feature branch started:

  ```
  git checkout master
  git pull origin master
  git checkout -b feature/my-feature-branch
  ... write lots of code, many commits ...
  ```

  When you are ready to have code reviewed:

  ```
  git checkout master
  git pull origin master
  git checkout feature/my-feature-branch
  ```
  If this is a new branch and it hasn't been previously pushed you may ...
  ```
  git rebase -i master
  ```
  This will allow you to squash and clean up your commits before code review for easier diffs/discussion ... otherwise ...
  ```
  git merge origin master
  ```
  Then ...
  ```
  git push origin feature/my-feature-branch
  ```
  Then create a PR in github.

  Once you are done with the code review, time to merge onto master!

### <a name="collaboration--git-naming">Branch Naming</a>

  `prefix/descriptive-branch-name`

  #### Prefixing

  `hotfix` - quick fix which will be merged directly into master, bypassing the review process

  `feature` - branch for building out a new feature, goes through normal review process before being merged into master

  `fix` - fix to be optionally merged into `staging` branch for deployment to staging for testing before being merged into `master` for deployment

# <a name="travis">Travis CI</a>

### <a name="travis--getting-started">Getting Started</a>

  1) Log in to [Travis CI](https://travis-ci.com/) - If you have a github account linked to P'unk Ave, you'll have the same permissions to access repos in the Travis UI
  2) Enable Travis for the repo you want to set up. This will automatically "do the things" to the repo (add an SSH key and enable builds).
  3) Add a `.travis.yml` file to your project. Basic config might look something like this:
  ```
  language: node_js
  node_js: '4.6.0'
  cache:
    directories:
      - node_modules
  before_script:
    - npm install
  script:
    - gulp build
  ```
  All sorts of options here: [https://docs.travis-ci.com/](https://docs.travis-ci.com/)
  4) Build away.
