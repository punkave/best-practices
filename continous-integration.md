# Continuous Integration

## Table of Contents
1. [Setup](#setup)
2. [Adding a status badge](#adding-a-status-badge)


## Setup

1. Head over to [CircleCI](https://circleci.com/add-projects/gh/punkave) - If you have a github account linked to P'unk Ave, you'll have the same permissions to access repos in the Circle UI
2. Click the `Set Up Project` button for the repo you want to set up. This will tell you to add a `config.yml` file for your project in a `.circleci` folder. An example of this config can be found in our [Client boilerplate](https://github.com/punkave/client-boilerplate/blob/master/.circleci/config.yml).
3. Once the config is committed and pushed, click the `Start Building` button back in the Circle UI.
4. Builds will happen on push.

## Adding a status badge

1. Click the gear icon on the project page that you want to set up a badge for `ex: https://circleci.com/gh/punkave/MYPROJECT`.
2. Click the `Status Badges` link under `Notifications` in the `Project Settings` side menu.
3. Select what branch you want to generate a badge for (usually master).
4. Generate an API token by following the `add one with status scope` link under `API Token` and clicking the `Create Token` button. You can label it whatever you like (`Status` or something similar is probably apt).
5. Go back to the `Status Badges` page and copy the embed code markdown and add it to your project readme.

## Permissions errors in builds

Occasionally, you may see an error on a build such as:
`sh: 1: concurrently: Permission denied`
This, or any permission denied error is likely a caching error (the vm user is different than cached version of npm dependencies).

To remedy this, you have to clear the npm cache. There are two ways to do this:
1. Update something in package json
2. Better option: update env variable in circle

See `client-boilerplate` for [circle related config](https://github.com/punkave/client-boilerplate/blob/master/.circleci/config.yml).
