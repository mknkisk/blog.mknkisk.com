---
title: 'MongoDB : Replica Set の参照設定'
date: 2018-06-04T01:36:29+09:00
draft: false
---

📝MongoDB のドキュメントを箇条書きしていく。随時更新

## Env / Versions

* MongoDB v3.6

## 参照の優先度設定

* ⚠️ Primary から Secondary への非同期レプリケーションには遅延が発生するため、古いデータが返される可能性がある

## 参照のモード

* 各 MongoDB ドライバーは以下の５つのモードをサポートしている

### primary

* Primary からのみ参照する
* Primary が使用できない場合はエラーになる
* デフォルト

### primaryPreferred

* Primary から参照するが使用できない場合は Secondary から参照する

### secondary

* Secondary からのみ参照する
* 使用可能な Secondary が無い場合はエラーになる

### secondaryPreferred

* Secondary から参照するが使用可能な Secondary が１台もない場合は Primary から参照する

### nearest

* ネットワークレイテンシが最も低いメンバーから参照する
* Primary, Secondary 問わない

## サーバ選択アルゴリズム

* サーバーの選択は操作ごとに1回発生し、参照設定とlocalThresholdMS の設定によって決められる
* サーバ選択アルゴリズムは各クライアントで実装されている
* secondary または secondaryPreferred モードを指定した場合 * 最も低いレイテンシ + 15msec
    以内のサーバを「使用可能」とする
  * 15msec はデフォルト値であり変更可能
  * 「使用可能」なサーバ群からランダムで接続先が決定される

## Links

* https://docs.mongodb.com/manual/core/read-preference/
* https://docs.mongodb.com/manual/core/read-preference-mechanics/
* [Manning | MongoDB in Action, Second Edition](https://www.manning.com/books/mongodb-in-action-second-edition)
