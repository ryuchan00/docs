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
