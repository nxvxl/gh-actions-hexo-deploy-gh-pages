# Hexo Github Action

GitHub Action for building and publishing Hexo-built site.

Inspired by [BryanSchuetz/jekyll-deploy-gh-pages](https://github.com/BryanSchuetz/jekyll-deploy-gh-pages)

## Secrets

- `GIT_DEPLOY_KEY` - *Required* your deploy key which has **Write access**

## Create Deploy Key

1. Generate deploy key `ssh-keygen -t rsa -f hexo -q -N ""`
1. Then go to "Settings > Deploy Keys" of repository
1. Add your public key within "Allow write access" option.
1. Copy your private deploy key to `GIT_DEPLOY_KEY` secret in "Settings > Secrets"

## Environment Variables

- `GITHUB_BRANCH` : **optional**, default is **gh-pages**

- `GITHUB_REMOTE_REPOSITORY` : **optional**, default is **GITHUB_REPOSITORY**


## Example

**push.yml** (New syntax)

**Deploy to gh-pages branch**: (under same repo)

- Note: put the `CNAME` file within your domain name inside `static` folder of compiling branch (master)

```yaml
name: Deploy to GitHub Pages

on: push

jobs:
  hexo-deploy-gh-pages:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: hexo-deploy-gh-pages
      uses: khanhicetea/gh-actions-hexo-deploy-gh-pages@master
      env:
        GIT_DEPLOY_KEY: ${{ secrets.GIT_DEPLOY_KEY }}
        HUGO_VERSION: "0.53"
```


**Deploy to Remote Branch**:

```yaml
name: Deploy to Remote Branch

on:
  push:
    branches:
      - master
      
jobs:
  hexo-deploy-gh-pages:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@master
    - name: hexo-deploy-gh-pages
      uses: khanhicetea/gh-actions-hexo-deploy-gh-pages@master
      env:
        GITHUB_REMOTE_REPOSITORY: <username>/<remote_repository_name>
        GITHUB_BRANCH: <custom_branch>
        GIT_DEPLOY_KEY: ${{ secrets.GIT_DEPLOY_KEY }}
        HUGO_VERSION: "0.58.3"
```

**Note**: make sure to add `GIT_DEPLOY_KEY` in secrets of mentioned `GITHUB_REMOTE_REPOSITORY`

**main.workflow** (Old syntax - deprecated)

```hcl
workflow "Deploy to GitHub Pages" {
  on = "push"
  resolves = ["hexo-deploy-gh-pages"]
}

action "hexo-deploy-gh-pages" {
  uses = "khanhicetea/gh-actions-hexo-deploy-gh-pages@master"
  secrets = [
    "GIT_DEPLOY_KEY"
  ]
  env = {
    HUGO_VERSION = "0.53"
    GITHUB_BRANCH = "master"
  }
}
```

## Example site

- https://github.com/khanhicetea/.com

## Other actions

- RPC Ping (Ping Search Engine) : https://github.com/khanhicetea/gh-actions-rpc-ping

## LICENSE

Copyright (c) 2019

Licensed under the MIT License.
