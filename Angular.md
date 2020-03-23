## nodenvのインストール

今回brewでインストールされたnode-buildではnodeの最新版が取得できなかったのでnode-buildは手動で入れた。

https://github.com/nodenv/nodenv#installation

https://github.com/nodenv/node-build#installation

```
brew install nodenv
mkdir -p "$(nodenv root)"/plugins
cd ~/.nodenv
cd plugins/node-build
git clone https://github.com/nodenv/node-build.git "$(nodenv root)"/plugins/node-build
git pull
nodenv install --list
nodenv install 12.16.1
nodenv local 12.16.1
```

## AngularMaterial

https://v5.material.angular.io/

## Angularの日本語公式ドキュメント

https://angular.jp/
