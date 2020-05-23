---
title: 'MongoDB データモデルデザイン'
date: 2018-06-04T23:51:00+09:00
draft: false
---

MongoDB 公式ドキュメントから抜粋

##Env / Versions

* MongoDB v3.6

## Embedded Data Models (埋め込みモデル)

Example:

```
{
  _id: <ObjectId1>,
  username: "123xyz",
  contact: {
    phone: "123-456-7890",
    email: "xyz@example.com"
  },
  access: {
    level: 5,
    group: "dev"
  }
}
```

* 非正規化モデル
* より少ないクエリとアップデートで済む
* 次のような時に使用する
 ・エンティティ間に「包含」関係がある。1対1の関係
 ・エンティティ間に1対多の関係がある。子ドキュメントは常に親ドキュメントのコンテキストで表現される
* 🙆‍♀️ Strengths
 ・単一のデータベースから関連するデータを取得するのと同じように、参照操作のパフォーマンスを向上させうる
 ・1回のアトミックな更新操作で関連するデータを更新することができる
* 🤦‍♀️ Weaknesses
 ・関連データをドキュメントに埋め込むと、作成後にドキュメントサイズが大きくなる
 ・MMAPv1ストレージエンジンを使用すると、ドキュメントの増大が書き込みのパフォーマンスに影響を与え、データの断片化を招く可能性がある
 ・📝 ドキュメントのサイズは最大で 16 megabytes

## Normalized Data Models (参照モデル)

Example:

```
// user document
{
  _id: <ObjectId1>,
  username: "123xyz"
}

// contact document
{
  _id: <ObjectId2>,
  user_id: <ObjectId1>,
  phone: "123-456-7890",
  email: "xyz@example.com"
}

// access document
{
  _id: <ObjectId3>,
  user_id: <ObjectId1>,
  level: 5,
  group: "dev"
}
```

* 正規化モデル
* 次のような時に使用する
 ・embedded による読み込みのパフォーマンスのメリットがデータが重複してしまうデメリットを上回らないとき
 ・より複雑な多対多の関係を表現するとき
 ・大きな階層化されたデータセットをモデル化するとき
* 参照モデルは埋め込みモデルよりもフレキシブル。
* ただし、クライアント側アプリケーションは、参照モデルを解決するためにフォローアップクエリを発行する必要がある。
 ・ 📝ドキュメント間の関連性の解決はクライアント側が担う(JOINとかないので、複数クエリの発行が必要)
* 正規化されたデータモデルでは、サーバーへのより多くのラウンドトリップが必要になる可能性がある

## Links

[Data Model Design — MongoDB Manual](https://docs.mongodb.com/manual/core/data-model-design/)
