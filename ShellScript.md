## フィールドのnカラム以降を切り出す

`ls -l` の結果の4つ目以降のカラムを切り出す。

```
ls -l | cut -d ' ' -f -4
```

参考文献

[awk で指定した n カラム目以降を出力する](https://blog.cles.jp/item/8437)

## tmuxでペインレイアウトを変更する

[tmuxでペインレイアウトを変更する - Qiita](https://qiita.com/tortuepin/items/1acbc7b0e749189a33b9)

## zsh_historyのバックアップを取る

```
#!/bin/bash
# エラーは捨てる
mkdir -p ~/zsh_history_backup 2>/dev/null
cp -p ~/.zsh_history ~/zsh_history_backup/zsh_history_`date "+%Y%m%d"`
```
