---
title: 'Setup a new Mac (El Capitan)'
date: 2015-11-18T01:32:12+09:00
draft: false
---

## Install homebrew

```
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

ref: http://brew.sh/index_ja.html

## Install fish

See my other post -> [Switching from zsh to fish](/posts/switching-from-zsh-to-fish/)

## Install brew-file

```
$ brew install rcmdnk/file/brew-file
```

ref: http://rcmdnk.github.io/blog/2015/06/30/computer-mac-brew-file-homebrew/
ref: https://github.com/rcmdnk/homebrew-file

## Install applications & libraries by homebrew & homebrew cask

```
$ mkdir work; cd work
$ git clone git@github.com:mknkisk/Brewfile.git
$ echo "set -x HOMEBREW_BREWFILE /Users/keisuke/work/Brewfile/Brewfile" >> ~/.config/fish/config.fish
$ brew file install
```

## Install applications from App Store

* Xcode
* Twitter
* Kobito

## Install other applications

* Clipy [https://clipy-app.com/]
* f.lux [https://justgetflux.com/]

## Auto start

```
$ ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents
$ ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents
$ ln -sfv /usr/local/opt/postgresql/*.plist ~/Library/LaunchAgents
```

## Install a font Ricty

```
$ cp -f /usr/local/Cellar/ricty/3.2.4/share/fonts/Ricty*.ttf ~/Library/Fonts/
```

## Install atom packages

```
$ apm stars --install
```

ref: http://mae.chab.in/archives/2605

## Install nodebrew

```
$ curl -L git.io/nodebrew | perl - setup
$ vim .zshrc
$ echo "set -x PATH $HOME/.nodebrew/current/bin $PATH" >> ~/.config/fish/config.fish
$ source ~/.config/fish/config.fish
$ nodebrew ls-all
$ nodebrew install-binary v0.12.7
$ nodebrew user v0.12.7
```

ref: https://github.com/hokaccha/nodebrew

## Install Ruby

```
$ CONFIGURE_OPTS="--disable-install-rdoc" rbenv install 2.3.0
```

## Setup PostgreSQL

```
$ initdb /usr/local/var/postgres
$ echo "set -x PGDATA /usr/local/var/postgres" >> ~/.config/fish/config.fish
```

## Rails trouble shooting

bundle install errors

```
$ bundle config build.therubyracer --with-v8-dir
$ bundle config build.libv8 --with-system-v8
$ bundle config build.eventmachine --with-cppflags=-I/usr/local/opt/openssl/include
```

or

```
$ bundle update libv8
```

## History

* 2016/04/06 * Switching from zsh to fish
