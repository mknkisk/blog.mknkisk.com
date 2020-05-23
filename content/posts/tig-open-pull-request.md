---
title: '[tig] Browse the Github pull request'
date: 2016-03-29T02:03:07+09:00
draft: false
---

Browse the Github pull request that includes selected commit

## Required

* [jonas/tig](https://github.com/jonas/tig)
* [github/hub](https://github.com/github/hub)

## open-pr command

```
$ vim open-pr
$ chmod +x open-pr
```

<script src="https://gist.github.com/mknkisk/381acbb8c87f67df5e3f.js"></script>
ref: https://gist.github.com/mknkisk/381acbb8c87f67df5e3f

Browse Github pull request page

```
$ open-pr {commit id}
```

## Custom bindings to tig
custom bindings to tig in your ~/.tigrc

```diff
+ bind generic P @open-pr %(commit)
```

## Refs

* [GitベースのコードリーディングTips - クックパッド開発者ブログ](http://techlife.cookpad.com/entry/2015/11/17/151426)
* [tig から当該コミットがマージされた Pull Request 画面を開く - Qiita](http://qiita.com/tmtysk/items/fd6592eb859e4523cd64)
* [Find the github pull request for a commit](http://joey.aghion.com/find-the-github-pull-request-for-a-commit/)
