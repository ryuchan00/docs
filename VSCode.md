## Tmuxがなぜかうまく起動しない

tmuxの起動条件は.bashrcより以下の通り

```
# tmuxをターミナル起動時に開く
if [ $SHLVL = 1 ]; then
tmux
fi
```

＄SHLVLが関係しているのではないかと思い、VSCode上のシェルレベルを確認すると４と表示された。

```bash
❯ echo $SHLVL
4
```

なぜこうなるのかというと、VSCodeのシェルは、レベル1のシェルの上にVSCodeの設定を読み込ませているようだ。したがってその設定をOFFにすれば良い。VSCodeでOepn Setteing(JSON)を選択して、json形式では編集を行う。以下のように編集すれば良い。

```json
{
    "window.zoomLevel": 0,
    "terminal.integrated.rendererType": "dom",
    "terminal.integrated.inheritEnv": false
}
```

`"terminal.integrated.inheritEnv": false` で環境を受け継ぐことをやめさせることができた。

## 参考資料

[VS Codeのサイドバーの表示をキーボードで切り替えるには：Visual Studio Code TIPS - ＠IT](https://www.atmarkit.co.jp/ait/articles/1807/13/news032.html)
