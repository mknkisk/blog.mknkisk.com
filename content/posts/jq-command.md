---
title: 'jqコマンドを使ってみた'
date: 2013-06-28T02:03:00+09:00
draft: false
---

jqコマンドなるものがあるそうなので入れてみました。

* [jq - GitHub Pages](http://stedolan.github.io/jq/)

参考にさせて頂いたのはこちらの記事です。

* [jqコマンドが実は高性能すぎてビビッた話](http://beatsync.net/main/log20130428.html)

jqはコマンドラインでJSONデータを扱うのに便利なコマンドです。

> jq is a lightweight and flexible command-line JSON processor.

## インストール

* MacBook Air Mid 2011
* Mac OS X 10.8.4
* Homebrew 0.9.4
* jq version 1.3

Mac なら homebrew を使うのが簡単そうです。

```
$ brew search jq
jq
$ brew install jq
$ jq --version
jq version 1.3
```

これで終わりです。簡単ですね。
ソースコードからのビルドやLinux, Windowsに関しては [公式ドキュメント](http://stedolan.github.io/jq/download/) をご覧ください。

## サンプルのJSONを用意する

JSONファイルを用意します。
jqの動きをざっと知りたいので簡素なJSONを。

```
$ cat sample.json
{"items":[{"name":"kirino"},{"name":"kuroneko"}]}
```

改行もインデントも無いフラットなJSONです。

## jqを使ってみる

用意したサンプルのJSONファイルを使ってjqの動きを確認してみます。

## JSON全体を読みやすく変換

"." を指定すると入力で与えられたJSONを全て出力します。

```
$ cat sample.json | jq "."
{
  "items": [
    {
      "name": "kirino"
    },
    {
      "name": "kuroneko"
    }
  ]
}
```

おぉ!!改行, インデントされて非常に見やすくなりました。
さらにコンソール出力するとデフォルトでシンタックスハイライトされます。
ハイライトをオフにする場合は -M オプションを指定します。

## フィルターを使う

jqの便利なところの1つに入力されたJSONから指定したプロパティのみ取得できることがあります。
サンプルのJSONから items の 2つめの要素のみ出力してみましょう。

".items[1]" を指定します。

```
$ cat sample.json | jq ".items[1]"
{
  "name": "kuroneko"
}
```

次に指定したプロパティの値のみ出力してみます。

```
$ cat sample.json |jq ".items[].name"
"kirino"
"kuroneko"
```

## おわりに

基礎の基礎のみ紹介しました。自分のための備忘録です。
jq には他にもJSONオブジェクトを生成したり計算させたり、関数を使うこともできます。
[公式マニュアル](http://stedolan.github.io/jq/manual/) には凡例つきで非常に丁寧に説明されているのでぜひご覧下さい。

JSONは python などでも簡単に扱うことができますが curl などで Web API から取得した
JSON の結果をちょっと確認したい。みたいな時には便利そうです。
複雑なフィルター条件になってきたら python などでパースするようにすると良さそうです。

実際に使う機会があればまた書こうと思います。

余談
[俺妹](http://www.oreimo-anime.com/)もTV放送は最終回ですか :'-(
8月の最終話配信を楽しみにしたいと思います :-D
