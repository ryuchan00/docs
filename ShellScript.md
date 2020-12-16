## フィールドのnカラム以降を切り出す

`ls -l` の結果の4つ目以降のカラムを切り出す。

```sh
ls -l | cut -d ' ' -f -4
```

参考文献

[awk で指定した n カラム目以降を出力する](https://blog.cles.jp/item/8437)

## tmuxでペインレイアウトを変更する

[tmuxでペインレイアウトを変更する - Qiita](https://qiita.com/tortuepin/items/1acbc7b0e749189a33b9)

## zsh_historyのバックアップを取る

```sh
#!/bin/bash
# エラーは捨てる
mkdir -p ~/zsh_history_backup 2>/dev/null
cp -p ~/.zsh_history ~/zsh_history_backup/zsh_history_`date "+%Y%m%d"`
```

## 複数のファイルをまとめて、1つのファイルにする

hoge.txtとfuga.txtをbar.txtというファイルにまとめる

```sh
cat hoge.txt fuga.txt > bar.txt
```

[【 複数ファイルをつなげて，1つのファイルにする 】 | 日経クロステック（xTECH）](https://xtech.nikkei.com/it/article/COLUMN/20060228/231150/)

## .gz ファイルの中身を検索する

## 複数のファイルをまとめて、1つのファイルにする

`zgrep` を使用する。 `grep` だとうまくいかないので注意する。

## 画像に文字を入れる

画像のテストをしたいときに使用する。ImageMagicを使用して作成する。

1200x1200の画像で、真ん中に「Dummy」文字列を挿入する例、色はカラーコードも使用可能。

```sh
convert -size 1200x1200 -gravity center -font AndaleMono -fill blue -background gray -pointsize 64 label:Dummy 1200x1200_dummy.jpg
```

[ImageMagickで画像に文字を描画 – エラーの向こうへ](https://tech.mktime.com/entry/145)

## 複数サーバーにコマンドを順次実行する

for文で回して実行する。

```sh
#!/bin/bash

HOSTS=('192.178.0.1' '192.178.0.2')

for i in ${HOSTS[@]}
do
ssh $i 'grep hoge ~/src/hoge_*.log'
done
```

## Pipfileとのバージョンが異なるためエラーになる

```
Warning: Your Pipfile requires python_version 3.7.7, but you are using 3.7.6 (/Users/fuga/.local/share/v/c/bin/python).
  $ pipenv --rm and rebuilding the virtual environment may resolve the issue.
  $ pipenv check will surely fail.
fish: 'pipenv run fab dev-update2' terminated by signal SIGABRT (Abort)
```

こんな感じでエラーが出た。Pipfileのpythonのバージョンをインストールすれば解決した。

```sh
pip install pipenv
```

[Pythonで、Pipenvを使う - Narito Blog](https://blog.narito.ninja/detail/58/)

## Macのterminalの2バイト文字が化ける

[terminalで日本語が文字化けする - Apple コミュニティ](https://discussionsjapan.apple.com/thread/10147279)

## ファイルの文字コードを変換する

nkfコマンドを使用する

```
~/Downloads
# -sオプションはShift-Jisに変換する、--cp932オプションを使用することで、cp932に準拠したShift-Jisになる
❯ nkf -s --cp932 --overwrite query_result_fail.csv

~/Downloads
# 変換できているか確認する、-gオプションを使用する
❯ nkf -g query_result_fail.csv
Shift_JIS
```
