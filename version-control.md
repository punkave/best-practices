# Version Control

We use Git for version control and GitHub for hosting repositories.

## Table of Contents
1. [Workflow](#workflow)

## Workflow

<a name="workflow"></a><a name="3.1"></a>
[3.1](#workflow) Gitflow

We use [gitflow workflow](http://nvie.com/posts/a-successful-git-branching-model/), a branching model designed around project releases. Each repository should have two historical branches, `master` and `develop`.

`origin/master` is the "production-ready" state of the application. `origin/develop` is stable code ready for a release that runs in parallel with `master`.

When working off of the `develop` branch use `feature` branches prefixed by "feature". For example, `feature/my-feature-branch`. When your feature is complete and ready for review you need to make a pull request to merge your code into `develop`. Your code should be passing all tests if applicable. If the code is front-end heavy the UI should be passing in all supported browsers and devices and there should be a link to the UI documented in the pull request. Your code must be reviewed and approved by another member of the team.

When ready to make a release to production you should use a `release` branch off of `develop`, prefixed by "release". For example, `release/1.0.0`. Your release branch should be named using [semantic versioning](http://semver.org/) and this version should be the same as your version number in `package.json`. A pull request should be made to merge your release into `master`.

**[⬆  back to top](#table-of-contents)**
