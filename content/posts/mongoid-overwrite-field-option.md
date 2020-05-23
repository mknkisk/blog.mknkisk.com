---
title: 'Mongoid フィールド定義の上書きを設定する'
date: 2016-07-23T16:40:17+09:00
draft: false
---

## Env

* Rails 4.2.7
* Mongoid 5.1.1

## 結論

警告を消したいなら、以下のように `overwrite` オプションを追加することで消すことができます。

```
field :_id, type: String, overwrite: true, default: -> { generate_id }
```

以下では設定前後の比較やログの確認をしています。

## 警告メッセージを確認する

フィールドを重複して定義していると警告がログに出力されます。
コーディングミスにより重複している場合はこのログで気づけるわけです。

ですが、例えば `_id` フィールドの採番ルールをカスタマイズしたい場合など、フィールドを再定義したい場合があります。

app/models/user.rb

```rb
class User
  include Mongoid::Document
  include Mongoid::Timestamps

  field :_id, type: String, default: -> { generate_id }
  field :created_at, type: Time, default: -> { Time.current }
end
```

Rails console

```
$ rails c
> User.first
```

log/development.log

```
Overwriting existing field _id in class User.
Overwriting existing field created_at in class User.
```

`_id` フィールドは Mongoid::Document を、 `created_at` フィールドは Mongoid::Timestamps
をインクルードした際に既に定義されているため、警告が出ます。

(実際には created_at を上書きしたいことって無いと思いますが)

## 上書き対象であることを定義に追加する

コーディングミスではなく、再定義したいがためのフィールド定義なので、上書き対象であることを明示的に指定してあげれば警告は出なくなります。

フィールド定義に `overwrite` オプションを追加します。

```diff
- field :_id, type: String, default: -> { generate_id }
- field :created_at, type: Time, default: -> { Time.current }
+ field :_id, type: String, overwrite: true, default: -> { generate_id }
+ field :created_at, type: Time, overwrite: true, default: -> { Time.current }
```

これで警告は出なくなります :)

## overwrite オプション

`overwrite` オプションは Mongoid 4.0.0 から追加されています

* [mongoid/CHANGELOG.md](https://github.com/mongodb/mongoid/blob/7199e4eee458b8b6777babcb02fc0f01b501d50c/CHANGELOG.md#400)
* [Add overwrite option to field #3146](https://github.com/mongodb/mongoid/pull/3146)
