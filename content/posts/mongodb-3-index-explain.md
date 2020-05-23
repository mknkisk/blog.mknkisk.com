---
title: 'MongoDB 3.x インデックス生成前後のexplain()結果を読む'
date: 2015-05-10T15:16:00+09:00
draft: false
---

## はじめに

MongoDB 3.0 から explain() の出力結果が変わり、読み解くのに時間がかかってしまいました。
今回はインデックスの生成前後で explain() の結果がどう変わるかを確認してみます。

## 環境

* Mac OS X 10.10.3
* MongoDB 3.0.2
* MongoDB storage engine: mmapv1 (default)

## サンプルデータの準備

## DBを用意

```
$ mongo

> use sample_db
switched to db sample_db
```

## サンプルデータ追加

とりあえず10万件のドキュメントを生成しておきます。


```
> for (var i=0; i < 100000; i++) {
... db.items.insert({ name: 'item_' + i, price: 100 + i })
... }

> db.items.count()
100000
```

## インデックスが無い状態

itemsコレクションから119円の商品を探すクエリを実行します。

```
> db.items.find({price: 119}).explain("executionStats")
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "sample_db.items",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"price" : {
				"$eq" : 119
			}
		},
		"winningPlan" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"price" : {
					"$eq" : 119
				}
			},
			"direction" : "forward"
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 1,
		"executionTimeMillis" : 59,
		"totalKeysExamined" : 0,
		"totalDocsExamined" : 100000,
		"executionStages" : {
			"stage" : "COLLSCAN",
			"filter" : {
				"price" : {
					"$eq" : 119
				}
			},
			"nReturned" : 1,
			"executionTimeMillisEstimate" : 20,
			"works" : 100002,
			"advanced" : 1,
			"needTime" : 100000,
			"needFetch" : 0,
			"saveState" : 781,
			"restoreState" : 781,
			"isEOF" : 1,
			"invalidates" : 0,
			"direction" : "forward",
			"docsExamined" : 100000
		}
	},
	"serverInfo" : {
		"host" : "MacBook-Pro.local",
		"port" : 27017,
		"version" : "3.0.2",
		"gitVersion" : "nogitversion"
	},
	"ok" : 1
}
```

まずは `queryPlannner.winningPlan` に着目します。`"stage": "COLLSCAN"` となっています。
これは MongoDB 2.x の時の `BasicCursor` に相当します。

インデックスを使わずに全走査してることがわかります。

※ COLLSCAN => COLLECTION SCAN

実際に走査対象となったドキュメント数やクエリにかかった時間は
`queryPlannner.executionStats` を確認します。

項目 | 説明 | 値
-- | -- | -
"nReturned": 1 | 見つかったドキュメント数 | 1
"executionTimeMillis": 59 | 59 | 59 msec
"totalKeysExamined": 0 | 検索したインデックス数 | 0
"totalDocsExamined": 100000 | 検索したドキュメント数 | 100000

`totalDocsExamined` からも全ドキュメントが検索対象だったことがわかりますね

※ インデックス生成してないので当たり前ですが

## インデックス生成後

それではインデックスを生成してみます。

```
> db.items.createIndex({ price: 1 })
{
	"createdCollectionAutomatically" : false,
	"numIndexesBefore" : 1,
	"numIndexesAfter" : 2,
	"ok" : 1
}

> db.items.getIndexes()
[
	{
		"v" : 1,
		"key" : {
			"_id" : 1
		},
		"name" : "_id_",
		"ns" : "sample_db.items"
	},
	{
		"v" : 1,
		"key" : {
			"price" : 1
		},
		"name" : "price_1",
		"ns" : "sample_db.items"
	}
]
```

`price_1` という名前の priceフィールド昇順 のインデックスができました。

追記

> db.collection.ensureIndex() は MongoDB 3.0.0 で deprecated と
なりましたので db.collection.createIndex() に書き直しました。


参考: [db.collection.ensureIndex()](http://docs.mongodb.org/manual/reference/method/db.collection.ensureIndex/#db.collection.ensureIndex)

再度、items コレクションから119円の商品を探すクエリを実行します。

```
> db.items.find({price: 119}).explain("executionStats")
{
	"queryPlanner" : {
		"plannerVersion" : 1,
		"namespace" : "sample_db.items",
		"indexFilterSet" : false,
		"parsedQuery" : {
			"price" : {
				"$eq" : 119
			}
		},
		"winningPlan" : {
			"stage" : "FETCH",
			"inputStage" : {
				"stage" : "IXSCAN",
				"keyPattern" : {
					"price" : 1
				},
				"indexName" : "price_1",
				"isMultiKey" : false,
				"direction" : "forward",
				"indexBounds" : {
					"price" : [
						"[119.0, 119.0]"
					]
				}
			}
		},
		"rejectedPlans" : [ ]
	},
	"executionStats" : {
		"executionSuccess" : true,
		"nReturned" : 1,
		"executionTimeMillis" : 13,
		"totalKeysExamined" : 1,
		"totalDocsExamined" : 1,
		"executionStages" : {
			"stage" : "FETCH",
			"nReturned" : 1,
			"executionTimeMillisEstimate" : 0,
			"works" : 2,
			"advanced" : 1,
			"needTime" : 0,
			"needFetch" : 0,
			"saveState" : 0,
			"restoreState" : 0,
			"isEOF" : 1,
			"invalidates" : 0,
			"docsExamined" : 1,
			"alreadyHasObj" : 0,
			"inputStage" : {
				"stage" : "IXSCAN",
				"nReturned" : 1,
				"executionTimeMillisEstimate" : 0,
				"works" : 2,
				"advanced" : 1,
				"needTime" : 0,
				"needFetch" : 0,
				"saveState" : 0,
				"restoreState" : 0,
				"isEOF" : 1,
				"invalidates" : 0,
				"keyPattern" : {
					"price" : 1
				},
				"indexName" : "price_1",
				"isMultiKey" : false,
				"direction" : "forward",
				"indexBounds" : {
					"price" : [
						"[119.0, 119.0]"
					]
				},
				"keysExamined" : 1,
				"dupsTested" : 0,
				"dupsDropped" : 0,
				"seenInvalidated" : 0,
				"matchTested" : 0
			}
		}
	},
	"serverInfo" : {
		"host" : "MacBook-Pro.local",
		"port" : 27017,
		"version" : "3.0.2",
		"gitVersion" : "nogitversion"
	},
	"ok" : 1
}
```

`queryPlannner.winningPlan` を確認します。 `"stage": "IXSCAN"` に変わりました。
これは MongoDB 2.x の時の `BtreeCursor` に相当します。
INDEXを使用していることがわかります :)

※ IXSCAN => INDEX SCAN

ここでまた、 `queryPlannner.executionStats` を確認します。

項目 | 説明 | 値
-- | -- | -
"nReturned": 1 | 見つかったドキュメント数 | 1
"executionTimeMillis": 13 | 13 | 13 msec
"totalKeysExamined": 1 | 検索したインデックス数 | 1
"totalDocsExamined": 1 | 検索したドキュメント数 | 1

1ドキュメントを検索するための検索対象ドキュメントが1となり理想的ではないでしょうか :)

## おわりに

最低限見ておきたい項目について、インデックスの生成前後での差分を見てみました。
実際の運用においては1コレクションに1インデックスという事は少なく、複数のインデックスや
複合キーインデックスが生成されていると思います。

そうすると今回見ていった項目以外にも `rejectedPlans` で採用されなかったインデックスを確認したり
ソート指定した場合のインデックスの使われ方を確認したりといった事が必要となるでしょう。

それぞれの項目の説明についてはまた別途まとめましょうか。

## 参考

[Explain Results - MongoDB Manual 3.0.2](http://docs.mongodb.org/manual/reference/explain-results/)
