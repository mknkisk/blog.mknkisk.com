---
title: 'Sign in to Errbit with GitHub'
date: 2017-02-17T14:40:53+09:00
draft: false
---

## Introduction

I wrote about the introduction of Erribit in past postings.  
[How to install Errbit for Express bug tracking 🐛](http://blog.mknkisk.com/how-to-install-errbit-for-express-bug-tracking/)

This post is about how to configure Errbit to sign in with Github.

## ENV

* Mac OSX 10.12.3
* MongoDB 3.4.2
* Node.js 7.5.0
* Express 4.14.0

## Generate new GitHub access token

open https://github.com/settings/tokens

## Configure Errbit

```
$ less .env.default

GITHUB_AUTHENTICATION=true
GITHUB_ACCESS_SCOPE="['repo']"
```

```
$ vim .env

GITHUB_CLIENT_ID=<YOUR_GITHUB_CLIENT_ID>
GITHUB_SECRET=<YOUR_GITHUB_SECRET>

$ bundle exec rails s -b 0.0.0.0 -p 3001
```

## Add a New User to Errbit


Fill in the GITHUB LOGIN field with your github username.

## Sign in with GitHub

## Appendix : GitHub Organization

```
# Get Github Organization infomation
$ curl -H "Authorization: token {YOUR_GITHUB_TOKEN}" https://api.github.com/orgs/{YOUR_ORGANIZATION_NAME}

$ vim .env
GITHUB_ORG_ID=<YOUR_ORGANIZATION_ID>
```

> Only users of the specified GitHub organization can log in to Errbit through
GitHub. Errbit will provision accounts for new users.


source: https://github.com/errbit/errbit#configuring-github-authentication

## Link

* [errbit/errbit - Configuring GitHub authentication](https://github.com/errbit/errbit#configuring-github-authentication)
