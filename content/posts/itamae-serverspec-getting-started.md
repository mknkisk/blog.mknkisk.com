---
title: 'Itamae & Serverspec ことはじめ'
date: 2016-05-15T15:20:36+09:00
draft: false
---

[Itamae](https://github.com/itamae-kitchen/itamae) を使って Vagrant 環境を構築してみます。
また、構築した環境は [Serverspec](http://serverspec.org/) でテストします。

今回のサンプルコードは [GitHub](https://github.com/mknkisk/itamae-samples) にアップしました。

## Env

* Mac OS X El Capitan 10.11.4
* ruby 2.3.1
* rake 11.1.2
* itamae 1.9.6
* serverspec 2.34.0
* Vagrant 1.8.1

## Vagrant セットアップ

まずは Vagrant の設定を。

```
$ mkdir workdir; cd workdir
$ vagrant init
```

CentOS7 で試してみたことがあったので、今回は Ubuntu でいきます。

```
$ vagrant init ubuntu/trusty64; vagrant up --provider virtualbox
```

ref: https://git.io/vrsCY

起動した Vagrant にログインできることを確認。

```
$ vagrant ssh
```

## Itamae で Nginx をインストール

Itamae も Serverspec も Ruby の Gem で公開されています。
Bundler を使ってインストールします。

```
$ bundle init
$ vim Gemfile
$ # diff

+ gem 'itamae'
+ gem 'serverspec'

$ bundle install --path vendor/bundle --jobs 4
```

ref: https://git.io/vrsB4

Itamae には決まったディレクトリ構成はありませんが、推奨の構成があります。[Best Practice · itamae-kitchen/itamae
Wiki](https://github.com/itamae-kitchen/itamae/wiki/Best-Practice#directory-structure)

今回はこの構成で進めていきます。

```
$ bundle exec itamae init
$ tree
.
├── Gemfile
├── Gemfile.lock
├── Vagrantfile
├── cookbooks
└── roles
```

手始めに Nginx のインストールレシピを書いてみます。

```
$ vim cookbooks/nginx/default.rb
$ vim roles/web.rb
```

ref: https://git.io/vrsB4

このレシピを Vagrant に反映します。

```
$ # .ssh/config に Vagrant のホスト設定を追加
$ vagrant ssh-config --host localhost.ubuntu >> ~/.ssh/config
$
$ # Itamae を実行
$ itamae ssh --vagrant --host localhost.ubuntu roles/web.rb
 INFO : Starting Itamae...
 INFO : Recipe: /your/workdir/roles/web.rb
 INFO :   Recipe: /your/workdir/cookbooks/nginx/default.rb
```

Vagrant 上で起動を確認

```
$ vagrant ssh
vagrant@localhost:~$ service status nginx
status: unrecognized service
```

## Serverspec で Nginx 周りをテスト

さて、せっかくサーバのセットアップがコード化できたので、設定の確認作業もコードで管理したいところです。

Serverspec でテストコードを書いていきます。
Serverspec も Itamae 同様にディレクトリをどう構成するするかは自由ですがコマンドでテンプレートを作ることができます。
こちらもデフォルトの構成で進めます。

serverspec-init コマンドを使って対話式でテンプレートを作成します。

```
$ bundle exec serverspec-init
Select OS type:

  1) UN*X
  2) Windows

Select number: 1

Select a backend type:

  1) SSH
  2) Exec (local)

Select number: 1

Vagrant instance y/n: y
Auto-configure Vagrant from Vagrantfile? y/n: n
Input vagrant instance name: localhost.ubuntu
 + spec/
 + spec/localhost.ubuntu/
 + spec/localhost.ubuntu/sample_spec.rb
 + spec/spec_helper.rb
 + Rakefile
 + .rspec
```

ref: https://git.io/vrs7b

spec 配下に作成されたディレクトリはテスト対象のホスト名です。
テストコードを対象ホスト単位でまとめるのか、ロール(web や db など)でまとめるのかは自由です。
Rakefile のコードを修正することで自由に編成できます。

Serverspecのサイトにあるサンプルが参考になります。
ref: [Serverspec - Advanced Tips](http://serverspec.org/advanced_tips.html)

Nginx のインストールとサービス設定、そして 80 ポートを Listen してるかのテストをします。

ref: https://git.io/vrs7A

テストは Rake で実行します。Rake タスクは動的に定義されます。(Rakefile 参照)
定義された Rake タスクは rake -T で確認できます。

```
$ rake -T
rake spec:localhost.ubuntu  # Run serverspec tests to localhost.ubuntu
```

テストを実行します。

```
$ rake spec:localhost.ubuntu
/Users/keisuke/.rbenv/versions/2.3.1/bin/ruby -I/Users/keisuke/.rbenv/versions/2.3.1/lib/ruby/gems/2.3.0/gems/rspec-support-3.4.1/lib:/Users/keisuke/.rbenv/versions/2.3.1/lib/ruby/gems/2.3.0/gems/rspec-core-3.4.4/lib /Users/keisuke/.rbenv/versions/2.3.1/lib/ruby/gems/2.3.0/gems/rspec-core-3.4.4/exe/rspec --pattern spec/localhost.ubuntu/\*_spec.rb

Package "nginx"
  should be installed

Service "nginx"
  should be enabled
  should be running

Port "80"
  should be listening

Finished in 0.58268 seconds (files took 5.66 seconds to load)
4 examples, 0 failures
```

テスト成功です :)

実際にはテストコードを共通化したり、複数ホストにテストを同時実行したりと、もっとカスタマイズすることになるでしょう。
その辺も実装してみたので、また今度まとめたいですね。

## おわり

たまにしかやらないからこそ、コード化できていると良いですね。仕事でも導入しましたが、インフラの変更履歴も共有でき、何よりテストが自動化できたのには安心感が増します。

とりあえず、初めの1歩でした :P
