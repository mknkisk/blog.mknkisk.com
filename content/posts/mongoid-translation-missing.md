---
title: 'Mongoid エラーメッセージが翻訳されない時'
date: 2016-07-24T17:16:05+09:00
draft: false
---

## Env

* Rails 4.2.7
* Mongoid 5.1.1

## Mongoid のエラーメッセージを正常に出力する

英語以外のロケールで Mongoid を使用していると、 Mongoid のエラーが発生した際にそのエラーメッセージが翻訳できずに `translation missing` エラーが発生してしまいます。

```rb
Mongoid::Errors::DocumentNotFound:
translation missing: ja.mongoid.errors.messages.message_title:
  translation missing: ja.mongoid.errors.messages.document_not_found.message
translation missing: ja.mongoid.errors.messages.summary_title:
  translation missing: ja.mongoid.errors.messages.document_not_found.summary
translation missing: ja.mongoid.errors.messages.resolution_title:
  translation missing: ja.mongoid.errors.messages.document_not_found.resolution
from /Users/himeko/work/MyApp/vendor/bundle/ruby/2.3.0/gems/mongoid-5.1.1/lib/mongoid/criteria.rb:454:in `check_for_missing_documents!'
```

エラーメッセージは英語で構わないですし、翻訳できない場合は英語にするように設定してしまいます。

application.rb

```diff
config.i18n.default_locale = :ja
+ config.i18n.fallbacks = { ja: :en }
```

すると正しくエラーメッセージが表示されるようになります。

```rb
Mongoid::Errors::DocumentNotFound:
message:
  Document(s) not found for class User with id(s) {:id=>"1"}.
summary:
  When calling User.find with an id or array of ids, each parameter must match a document in the database or this error will be raised. The search was for the id(s): {:id=>"1"} ... (1 total) and the following ids were not found: {:id=>"1"}.
resolution:
  Search for an id that is in the database or set the Mongoid.raise_not_found_error configuration option to false, which will cause a nil to be returned instead of raising this error when searching for a single id, or only the matched documents when searching for multiples.
from /rails_root/vendor/bundle/ruby/2.3.0/gems/mongoid-5.1.1/lib/mongoid/criteria.rb:454:in `check_for_missing_documents!'
```
