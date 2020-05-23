---
title: 'Switching from zsh to fish'
date: 2016-01-13T02:23:19+09:00
draft: false
---

zsh -> fish

## Env

* fish v2.5.0
* fisherman v2.11.0

## Install fish

```
$ brew install fish
```

## Set default shell
/etc/shells

```diff
# last line
+ /usr/local/bin/fish
```

```
$ chsh -s /usr/local/bin/fish
```

## Install fisherman

```
$ curl -Lo ~/.config/fish/functions/fisher.fish --create-dirs git.io/fisher
```

refs: https://github.com/fisherman/fisherman#install

## Install theme: bira

```
$ fisher install omf/theme-bira
```

## Config

`$HOME/.config/fish/config.fish`

```
# vi mode
fish_vi_key_bindings

# rbenv
set RBENV_ROOT -x /usr/local/var/rbenv
rbenv init - | source

# nodebrew
set -x PATH $HOME/.nodebrew/current/bin $PATH

# abbr list
abbr -a be bundle exec
abbr -a g git
abbr -a gb git branch
abbr -a gc git checkout
abbr -a gs git status
abbr -a o open
abbr -a v vim
```

## Refs

* [fisherman/fisherman](https://github.com/fisherman/fisherman)
* [お手軽なzshからfish shellへの移行手順 - Qiita](http://qiita.com/sifue/items/20ae88730ab929ccf420)
* [詳解 fishでモダンなシェル環境の構築(fish,tmux,powerline,peco,z,ghq,dracula) - Qiita](http://qiita.com/susieyy/items/ac2133e249f252dc9a34)
* [oh-my-fish/Themes.md at master · oh-my-fish/oh-my-fish](https://github.com/oh-my-fish/oh-my-fish/blob/master/docs/Themes.md)

## History

* 2017/03/15
  * Update fisherman version 2.11.0
  * Update fisherman install command (remove homebrew command)
  * Update .config/fish/config.fish
  * Remove set fisher_home, set fisher_config
* 2016/05/25
  * Install fisherman with homebrew
  * Update .config/fish/config.fish
  * fish_vi_mode -> fish_vi_key_bindings (fish_vi_mode is
    deprecated)
  * set RBENV_ROOT
* 2016/02/20
  * fisherman theme: agnoster -> shellder
  * Add fisherman install command
