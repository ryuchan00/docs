# WSL2+Docker+Rails+PostgreSQL での環境構築

## WSL2の導入

[Windows 10 に WSL をインストールする | Microsoft Docs](https://docs.microsoft.com/ja-jp/windows/wsl/install-win10) をみて、ubuntuをインストールした。

## zshの導入

```
sudo apt-get install zsh
```

oh-my-zsh もインストールした。[^1]

```
 sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
 ```

## pecoの導入

pecoを導入する。[^2] (本当はv0.5.10にすればよかった。)

```
# 以下のページから最新バージョンをwget
# https://github.com/peco/peco/releases
wget https://github.com/peco/peco/releases/download/v0.5.7/peco_linux_386.tar.gz
tar xzvf peco_linux_386.tar.gz
cd peco_linux_386
# PATHの通っている所にpecoを配置
sudo cp peco /usr/local/bin
peco --version
```

ctrl+rでpecoを使用できるようにする。そのために.zshrcに以下のメソッドを定義する。

```.zshrc
function peco-select-history() {
 local tac
 if which tac > /dev/null; then
     tac="tac"
 else
     tac="tail -r"
 fi
 BUFFER=$(\history -n 1 | \
     eval $tac | \
     peco --query "$LBUFFER")
 CURSOR=$#BUFFER
}
zle -N peco-select-history
bindkey '^R' peco-select-history
```

## Dockerのインストール

https://hub.docker.com/editions/community/docker-ce-desktop-windows/ よりDocker Desktopをインストールした。[^3]そのあとに [ここのとおり](https://docs.docker.com/desktop/windows/wsl/#install) 「Use WSL 2 based engine」にチェックをした。また、WLSでDockerを使用できるように、Settings->Resources->WSL INTEGRATIONで 「Enable integration with my default WSL distro」にチェックをした。また自分の使用しているディストリビューションを有効にしておく。

## Railsのコンテナをたてる

Rails+Postgresqlの構成でコンテナを立てた。[^4]

Dockerfile

```Dokcerfile
# syntax=docker/dockerfile:1
FROM ruby:2.5
RUN apt-get update -qq && apt-get install -y nodejs postgresql-client
WORKDIR /myapp
COPY Gemfile /myapp/Gemfile
COPY Gemfile.lock /myapp/Gemfile.lock
RUN bundle install

# コンテナー起動時に毎回実行されるスクリプトを追加
COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

# イメージ実行時に起動させる主プロセスを設定
CMD ["rails", "server", "-b", "0.0.0.0"]
```

Gemfile

```Gemfile
source 'https://rubygems.org'
gem 'rails', '~>5'
```

空のGemfile.lockを作成する。

```
$ touch Gemfile.lock
```

> 次にエントリーポイントの実行スクリプトを生成して、Rails 特有の問題へ対処します。 これはサーバー内にserver.pidというファイルが先に存在していたときに、サーバーが再起動できなくなる問題を回避するものです。 このスクリプトは、コンテナーが起動されるたびに実行されることになります。

このスクリプトはentrypoint.shとする。

```sh
#!/bin/bash
set -e

# Rails に対応したファイル server.pid が存在しているかもしれないので削除する。
rm -f /myapp/tmp/pids/server.pid

# コンテナーのプロセスを実行する。（Dockerfile 内の CMD に設定されているもの。）
exec "$@"
```

docker-compose.ymlは以下の通りである。ここでつまったのが `volumes` を指定しなかったことである。WSLはWSL側とDocker側のuserが異なるため参考の通りにやっても権限でエラーが発生した。したがってvolumesを使用した。

```
version: "3.9"
services:
  db:
    image: postgres
    volumes:
      - pgdata:/var/lib/postgresql/data
    environment:
      POSTGRES_PASSWORD: password
  web:
    build: .
    command: bash -c "rm -f tmp/pids/server.pid && bundle exec rails s -p 3000 -b '0.0.0.0'"
    volumes:
      - .:/myapp
    ports:
      - "3000:3000"
    depends_on:
      - db
volumes:
  pgdata:
```

プロジェクトをビルドした。

```
$ docker-compose run --no-deps web rails new . --force --database=postgresql
$ sudo chown -R $USER:$USER .
$ docker-compose build
```

config/database.ymlを以下のように変更した。

```config/database.yml
default: &default
  adapter: postgresql
  encoding: unicode
  host: db
  username: postgres
  password: password
  pool: 5

development:
  <<: *default
  database: myapp_development


test:
  <<: *default
  database: myapp_test
```

```
$ docker-compose up
```

`localhost:3000` にアクセスして、Railsのスタートページが見れた。

[^1][Oh My Zsh - a delightful & open source framework for Zsh](https://ohmyz.sh/)
[^2][Ubuntu[](url)にpecoを導入する(for bash) (for fish) - Qiita](https://qiita.com/jagio/items/7b020df50c4b1bdc6ad0)
[^3][Docker Desktop WSL 2 backend | Docker Documentation](https://docs.docker.com/desktop/windows/wsl/)
[^4][クィックスタート: Compose と Rails | Docker ドキュメント](https://matsuand.github.io/docs.docker.jp.onthefly/samples/rails/)
