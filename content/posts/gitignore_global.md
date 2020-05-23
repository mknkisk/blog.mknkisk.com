---
title: 'gitignore_global - git Ignore only my settings'
date: 2017-02-24T12:19:48+09:00
draft: false
---

Git ignore locally files.

.DS_Store is only Mac user.
.vscode dir is only Visual Studio Code User.

```
$ git config --global core.excludesfile ~/.gitignore_global
$ vim ~/.gitignore_global

.vscode
.DS_Store
```
