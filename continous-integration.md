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
