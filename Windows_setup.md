```
インストールするディストリビューションを探す
> wsl --list --online
インストールできる有効なディストリビューションの一覧を次に示します。
'wsl --install -d <Distro>' を使用してインストールします。

NAME            FRIENDLY NAME
Ubuntu          Ubuntu
Debian          Debian GNU/Linux
kali-linux      Kali Linux Rolling
openSUSE-42     openSUSE Leap 42
SLES-12         SUSE Linux Enterprise Server v12
Ubuntu-16.04    Ubuntu 16.04 LTS
Ubuntu-18.04    Ubuntu 18.04 LTS

LTSが使い勝手が良いかなと思いubuntu20.04をinstall
> wsl --install -d Ubuntu-20.04

wsl2を標準にする
> wsl --set-default-version 2

wsl2か確認する
> wsl --status
既定のバージョン: 2

Linux 用 Windows サブシステムの最終更新日: 2021/09/24
Windows Subsystem for Linux カーネルは、'wsl --update' を使用して手動で更新できますが、システム設定が原因で自 動更新が発生することはありません。
カーネルの自動更新を受け取るには、 Windows Update の設定を有効にしてください:' Windowsの更新に、その他のMicrosoftの製品の更新情報を受け取る'。
詳細については、 https://aka.ms/wsl2kernel.
 を参照してください
カーネル バージョン: 5.10.16

wsl2でubuntuがinstallできなかった。エラーコードは0x800701bc
WslRegisterDistribution failed with error: 0x800701bc
いろいろ調べたがどうやらLinuxカーネルのアップデートが必要、なので手動でアップデートする
多分Windows Updateが挟まらないタイミングでWSLの機能を有効にしたため、Windowsの更新プログラムが当たっていなさそう
> wsl --update

これで動くようになった。
```

## RubyMineの設定

コマンドラインでUbuntuからWindowsのRubyMineを参照したいときは、エイリアスを追加する。

[コマンドラインインターフェース | RubyMine](https://pleiades.io/help/ruby/working-with-the-ide-features-from-command-line.html#standalone)

```
echo "alias rubymine='/mnt/c/Program\ Files/JetBrains/RubyMine\ 2021.2.2/bin/rubymine64.exe'"  >> ~/.zshrc
# プロジェクトルートに移動する
rubymine .
```
