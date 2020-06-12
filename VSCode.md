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

## ショートカットキー参考文献

[VS Codeのサイドバーの表示をキーボードで切り替えるには：Visual Studio Code TIPS - ＠IT](https://www.atmarkit.co.jp/ait/articles/1807/13/news032.html)

サイドバーを閉じるには command + b

[VSCodeでコード整形ツール(Fomatter)を使用する - Qiita](https://qiita.com/mgmgOmO/items/17438d45790635bd8dca)

コード整形は、shift + option + f

## Scrach file の作り方

untitleなnew fileを作成すれば、それがscrach fileになるとのこと。

[Ability to add scratch files in vscode 🔥 · Issue #60377 · microsoft/vscode](https://github.com/Microsoft/vscode/issues/60377)

## AutoSave

VSCodeは、手動保存しないとファイルが保存されない。なので自動保存の設定をsettings.jsonに付け足す。`afterDelay`にするとデフォルトで1000ms後に保存する事になる。

```json
"files.autoSave": "afterDelay"
```

[Basic Editing in Visual Studio Code](https://code.visualstudio.com/docs/editor/codebasics#_save-auto-save)

## ファイルオープン時に新しいタブで開く

```json
"workbench.editor.enablePreview": false
```

[VSCodeでファイルを開いた時にタブが上書きされる問題を回避する - Qiita](https://qiita.com/kgoto/items/265e3eb8a82778e33cfe)

## ファイル名検索

Macの場合は command + p

[Visual Studio Code チートシート - Qiita](https://qiita.com/aoinu/items/681f932ce0b162cd9520#%E3%82%B3%E3%83%9E%E3%83%B3%E3%83%89)

## カーソル行を削除する

shift + command + k

## タイトルバーにプロジェクトルートからのパスを表示させる

settings.jsonを以下のように編集する

```json
"window.title": "${dirty}${activeEditorMedium}${separator}${rootName}",
```

[VSCodeでタイトルにパスを表示してわかりやすくする - Qiita](https://qiita.com/akisx/items/08313fd5b47b5a45bb2f)

## ソフトラップ状態で表示する

settings.jsonを以下のように編集する

```json
"editor.wordWrap": "on" 
```

[How can I switch word wrap on and off in Visual Studio Code? - Stack Overflow](https://stackoverflow.com/questions/31025502/how-can-i-switch-word-wrap-on-and-off-in-visual-studio-code)

## 保存時に行末尾の空白を自動で消す

settings.jsonを以下のように編集する

```json
"files.trimTrailingWhitespace": true,
```

## 範囲を選択してコード整形を行う

> 整形したい範囲を選択して、
Cmd + K → Cmd + F

これで選択した範囲のみ整形される。

[Visual Studio Code + mac で非常に良く使うショートカット - Qiita](https://qiita.com/matsuyoro/items/bbf7ef7eb9f36b65cd27)
