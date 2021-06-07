# MySQL

## ユニーク制約の着脱

```sql
-- まずユニーク制約の確認を行う
show indexes from [table name];

```

[MySQL キー着脱メモ - Qiita](https://qiita.com/reneice/items/3d9064b4396f98302397)

[MYSQLでUNIQUE制約を外す方法 - Qiita](https://qiita.com/katsukii/items/992a04aaec5fc87919f3)

## MySQLのALTERは場合によっては時間がかかる

ALTER TABLEはテーブルの構造を変更するSQL、ALTERの種類によっては実行中に書き込みができなくなる。

### 実行中に書き込みできない&テーブルコピーする

ここでいうテーブルコピーとはテンポラリテーブルを使用して、新しいテーブルを作成して、リネームして元あるテーブルと置き換えることのこと。

- カラムのデータ型の変更
- 全文検索用のインデックスの追加
- プライマリキーの削除
- 文字コードの変更など

### 実行中に書き込みでいる＆テーブルコピーが必要

- カラムの追加、削除、並び替え
- カラムをNULLABLEにする
- 上記の逆

### 実行中に書き込みできる＆テーブルコピーが不要

- インデックスの追加
- カラムのでフォル値の設定
- オートインクリメント値の変更
- 外部キー制約の追加、削除

メタデータのロックについて

https://dev.mysql.com/doc/refman/5.6/ja/metadata-locking.html


## テーブルの結合で迷ったら

inner join, left join, right join

https://qiita.com/ngron/items/db4947fb0551f21321c0

innter join,right joinは新たに結合するテーブルが基準、新たに結合するテーブルにレコードがないと、レコードが落ちる。
left joinはその逆で、元にある結合先のテーブルが基準、結合先のレコードがないと新たに結合するテーブルのレコード情報は取得できない。

## カラムを場合分けで取得する

CASEを使用する。下記の例は、user_detailsテーブルのnameレコードがnullだった場合は、usersテーブルのnameを参照し、そうではなかったらuser_detailsテーブルのnameを参照して、nameカラムとして扱う。こうすることで、SQLで処理した後に改めて配列を探索する必要がなくなる。

```sql
SELECT
  CASE
    WHEN (user_details.name IS NULL) THEN users.name
    ELSE user_details.name
  END as name,
```

[全ては時の中に… : 【SQL】CASE式でNULLかどうかを判定する](http://blog.livedoor.jp/akf0/archives/51469844.html)

[MySQL CASE文でNULLの分岐はコツがいる - Qiita](https://qiita.com/ayies128/items/8d5ddb39af83fe5138ff)

## indexの付け方

> インデックスとは、データベース内から特定のデータを検索しやすくするための仕組みです。
インデックスにはいくつか方式がありますが、MySQLで使われているのはB-Treeという方式です。B-Treeの仕組みをざっくりと説明すると、インデックスを貼られたカラムは検索しやすいようツリー構造に加工され、テーブル本体とは別の場所に保存されます。検索する際にはその加工したデータの方を見に行くことで、高速化を図っているのです。
インデックスを貼るメリットは、先ほど述べたとおり検索の高速化です。適切なカラムにインデックスを貼っておくことにより、インデックスなしの状態よりかなり高速化することができる場合があります。ただしインデックスを貼る対象はなんでもいいというものではなく、どんなSQL文を多用する予定であるかを考え、実行計画を確認しながらきちんと検討する必要があります。

[MySQLでインデックスの追加・確認・削除を行う方法 | サービス | プロエンジニア](https://proengineer.internous.co.jp/content/columnfeature/6818)

- PRIMARY KEY
  - インデックスじゃない、重複不可
- UNIQUE KEY
  - インデックスである、重複不可
- KEY
  - インデックスである、重複できる

[プライマリキーとインデックスの違い - bi_naの日記](https://bi-na.hatenadiary.org/entry/20120218/1329530104)
[UNIQUE制約(ユニーク制約を設定する) | MySQLの使い方](https://www.dbonline.jp/mysql/table/index9.html)

## 外部キー制約

```sql
-- 書き方の例
FOREIGN KEY `item_id` (`item_id`) REFERENCES `items`(`id`) ON DELETE CASCADE
```

この時、itemsテーブルのidカラムにUNSIGNEDが設定してあり、item_idカラムにUNSIGNED(符号なし)がないとエラーになる。

[FOREIGN KEY制約(外部キー制約を設定する) | MySQLの使い方](https://www.dbonline.jp/mysql/table/index11.html)

[MySQL/SQL/外部キー/外部キーを無視してテーブルを更新する - yanor.net/wiki](https://yanor.net/wiki/?MySQL/SQL/%E5%A4%96%E9%83%A8%E3%82%AD%E3%83%BC/%E5%A4%96%E9%83%A8%E3%82%AD%E3%83%BC%E3%82%92%E7%84%A1%E8%A6%96%E3%81%97%E3%81%A6%E3%83%86%E3%83%BC%E3%83%96%E3%83%AB%E3%82%92%E6%9B%B4%E6%96%B0%E3%81%99%E3%82%8B)
