# Ruby on Rails

## migration漏れがないか確認する

```bash
bundle exec rake db:migrate:status
```

## ActiveModelのtakenのエラー

validateのユニーク制約に引っかかると taken になる。

[ActiveModel::Errorsのエラーメッセージをエラータイプにしたい - Qiita](https://qiita.com/xhnagata/items/c07be18de3f14adaa198)

## RSpecでのModelのvalidationエラーの確認をする

`.errors` にエラーオブジェクトが入っている。以下が使用例である。

```rb
> a = build(:user)
> a.valid?
=> false
> a.errors.error_info
> a.errors
> a.errors.messages
=> {:attribute=>[:blank, :blank]}
```

## 複数のレコードを作成する

`careate` や `build` にハッシュが要素の配列を渡してあげれば、複数レコードをinsertできたりする

```rb
class Person
  has_many :pets
end

person.pets.build
# => #<Pet id: nil, name: nil, person_id: 1>

person.pets.build(name: 'Fancy-Fancy')
# => #<Pet id: nil, name: "Fancy-Fancy", person_id: 1>

person.pets.build([{name: 'Spook'}, {name: 'Choo-Choo'}, {name: 'Brain'}])
# => [
#      #<Pet id: nil, name: "Spook", person_id: 1>,
#      #<Pet id: nil, name: "Choo-Choo", person_id: 1>,
#      #<Pet id: nil, name: "Brain", person_id: 1>
#    ]

person.pets.size  # => 5 # size of the collection
person.pets.count # => 0 # count from database
```

https://api.rubyonrails.org/classes/ActiveRecord/Associations/CollectionProxy.html#method-i-build

## CSRFの方法

CSRFの対策を有効にするには、 `protect_from_forgery` メソッドをcontrollerに書く。 `with:` オプションはCSRFトークンがinvalidだった場合に呼ぶメソッドである。下記の場合は、例外をあげるようにする。何も書かないと例外を上げずに終わるので注意すること。

```rb
protect_from_forgery with: :exception
```

[Rails セキュリティガイド - Railsガイド](https://railsguides.jp/security.html#csrf%E3%81%B8%E3%81%AE%E5%AF%BE%E5%BF%9C%E7%AD%96)

## findとfind_byの戻り値の違い

- `.find` はレコードが見つからない場合には例外を返す
- `.find_by` はレコードが見つからない場合はnilを返す

挙動の違いがあるのでエラーハンドリングに注意すること。

[【Rails】find・find_by・whereについてまとめてみた - Qiita](https://qiita.com/nakayuu07/items/3d5e2f8784b6f18186f2)

## リソースのまとめて削除

`destroy_all` メソッドを使用する。

[ActiveRecordにおけるdestroyとdestroy!の違い - Qiita](https://qiita.com/jnchito/items/3393c5c1a744199e128a)

## Railsのプレースホルダー

```rb
User.where('rank=?','diamond')
```

[Active Record のプレースホルダー付き条件を知る - Qiita](https://qiita.com/FumiyaShibusawa/items/5dd3633e91b69d04150b)

## routes.rbの書き方

```rb
# ネームスペースの切り方、ネストで囲まれた部分が/v1/以下のリソースになる
namespace :v1, defaults: {format: :json} do
  # UsersControllerに対してのパスを定義すると、resourcesを使用することとなる
  # この場合は、showなどはGET /v1/users/1のようにuser idがパスに入るようになる
  resources :users, only: [:index, :create, :show] do
    # Userモデルに従属しているScoreモデルのエンドポイントを作成するなら以下のような表現になる
    # GET /v1/users/1/scores/1
    resources :scores, only: [:index, :create, :show] do
    end

    # user idを必要としないものは、collectionを用いる
    # POST /v1/users/batch_delete
    collection do
      post :batch_delete
    end
  end
end
```


## エラーキャッチの方法

rescueでもいいが洗礼された方法として `rescue_from` が存在する。

```rb
class ApplicationController < ActionController::Base
  rescue_from ActiveRecord::RecordNotFound, with: :record_not_found

  private

    def record_not_found
      render plain: "404 Not Found", status: 404
    end
end
```

ちなみにblockを渡すこともできる。こうすることによってエラーオブジェクトを活用することができる。

```rb
rescue_from ActiveRecord::RecordNotFound do |e|
  custom_render(e.model.constantize.model_name.human)
end
```

[Action Controller の概要 - Railsガイド](https://railsguides.jp/action_controller_overview.html#rescue-from)

## apiを作成するときのエラーレスポンスの例

社でエラーレスポンスを悩んだ時に良い例があった。titleにはエラータイトルを、detailにはタイトルに対しての詳細を書区と良いみたい。

```json
{
  "errors": [
    {"title": "not found", "detail": "user is not found"},
    {"title": "...", "detail": "..."},
  ]
}
```

## エラーモジュールの設計

最低限のエラーモジュールは以下のような感じ、これをApplicationControllerなどに `include ErrorRenderable` する。

```rb
module ErrorRenderable
  extend ActiveSupport::Concern

  included do
    rescue_from ActiveRecord::RecordNotFound do |e|
      render_error({ title: :not_found, detail: :not_found, placeholders: { name: e.model.constantize.model_name.human } }, status: :not_found)
    end

    private

    def render_error(error, status:)
      errors = [error].flatten.map { |error|
        title, detail, placeholders = error.values_at(:title, :detail, :placeholders)
        {
          title: I18n.t(title, scope: [:errors, :responses, :title]),
          detail: detail_messages(detail, placeholders)
        }
      }

      render json: { errors: errors }, status: status
    end

    def detail_messages(detail, placeholders)
      case detail
      when Symbol
        I18n.t(detail, (placeholders || {}).merge(scope: [:errors, :responses, :detail]))
      else
        detail
      end
    end
  end
end
```

### e.model.constantize.model_name.human について

エラーレスポンス作成時に意味がわからなかったコードが `e.model.constantize.model_name` であった。 `constantize` メソッドは、レシーバーが文字列だと、その定数があるか調べて、定数を返す。つまり `e.model` が"User"だった場合は `e.model.constantize` は `User` になる。そこで特異メソッドが使えるようになる。 `modle_name.human` は下の例を見てもらうとActiveModelオブジェクトのインスタンス変数は色々持っているが、その中で、自分のリソース名を単数で持っている。

```rb
[2] pry(#<Api::OptionSetAssignmentRulesController>)> e.model.constantize.model_name
=> #<ActiveModel::Name:0x0000560f60c75518
 @collection="option_set_assignment_rules",
 @element="option_set_assignment_rule",
 @human="Option set assignment rule",
 @i18n_key=:option_set_assignment_rule,
 @klass=OptionSetAssignmentRule(id: integer, target_type: integer, target_value: string, sort_number: integer, created_at: datetime
 @name="OptionSetAssignmentRule",
 @param_key="option_set_assignment_rule",
 @plural="option_set_assignment_rules",
 @route_key="option_set_assignment_rules",
 @singular="option_set_assignment_rule",
 @singular_route_key="option_set_assignment_rule">
```

### I18n.tについて

```rb
        I18n.t(detail, (placeholders || {}).merge(scope: [:errors, :responses, :detail]))
```

`I18n.t` の第一引数は、ymlの中のキーであるが、第二引数がよくわかっていなかった。placeholders変数には `name:` のキーが含まれている。 `scope` は、I18nテンプレートの場所を指定している。この場合はerror->responses->detailになっている。指定しない場合は、トップレベルが参照される。

## Railsでのhttp status codeの一覧

[HTTP status code symbols for Rails](https://gist.github.com/mlanett/a31c340b132ddefa9cca)

## i18nの導入

config/application.rb

```rb
# デフォルトのロケーションを指定する
config.i18n.default_locale = :ja
# ロケーションファイルの場所を追加する
config.i18n.load_path += Dir[Rails.root.join('config/locales/**/*.yml')]
```

以下のような例で config/locales/ja.yml を追加する。

```yml
ja:
  activerecord:
    models:
      user: 使用ユーザー
  errors:
    responses:
      detail:
        not_found: "%{name}が見つかりませんでした"
      title:
        not_found: 見つかりませんでした
```

## レコード削除時に関連したレコードも一緒に削除する

関連に対して `dependent: :destroy` をつける。

```rb
# models/card.rb
has_many :cards_coupons, dependent: :destroy  #追加
has_many :coupons, through: :cards_coupons

# models/cards_coupon.rb
belongs_to :card
belongs_to :coupon, dependent: :destroy  #追加

# models/coupon.rb
has_many :cards_coupons
has_many :cards, through: :cards_coupons

```

[【Rails】レコード削除時に関連するテーブルのレコードも一緒に削除する方法 - Qiita](https://qiita.com/y-suna/items/67e13eb6072db93cf58d)

## ハッシュにキーをシンボルから文字列に変更する

```rb
{ a: 1, b: 2, c: 3 }.stringify_keys

# => {"a"=>1, "b"=>2, "c"=>3}
```

[Rails | ハッシュのキーを全てシンボルから文字列に変える ( stringify_keys / deep_stringify_keys ) - Qiita](https://qiita.com/Yinaura/items/4d999e0ee6b450f25ae7)

## whereの否定をする

```rb
# notを使用する
ids = [1,2]
# idが1,2以外のものが取得できる
User.where.not(id: ids)
```

[Rails whereの検索で否定を使う | Workabroad.jp](https://workabroad.jp/posts/2029)

## controllerでメソッド実行前に前処理をする

```rb
# update前にset_userメソッドが呼ばれる
before_action :set_user
# 前処理を実行するメソッドを限定する場合はonlyを指定する
before_action :set_user, only: [:update]
# ブロックを渡してあげても機能する
before_action do
  @user = User.find(params[:id])
end

def update
  @user.update!(params)
end

def set_user
  @user = User.find(params[:id])
```

## ViewテンプレートでのN + 1問題を解決する

jbuilderの実装例ではあるが、以下のように書くと `products` の数だけN+1になってしまう。

```jbuilder
json.products do
  json.array! @users.products do |product|
  json.extract! product, :id, :name
end
```

したがって、 `@user` オブジェクト作成時に、関連を一緒に取得してキャッシュしてしまった方が良い。方法としては `includes` や `joins` を使用する方法がある。僕はよくincludesを使用するのでその例を書く。

```rb
users = User.includes(:address, friends: [:address, :followers])
```

上の例ではUserと関連を持っているAddressモデルとFriendモデル、さらにFriendモデルと関連のあるAddresモデルとFollowerモデルをまとめて引ける。

[Active Record クエリインターフェイス - Railsガイド](https://railsguides.jp/active_record_querying.html#%E8%A4%87%E6%95%B0%E3%81%AE%E9%96%A2%E9%80%A3%E4%BB%98%E3%81%91%E3%82%92%E4%B8%80%E6%8B%AC%E3%81%A7%E8%AA%AD%E3%81%BF%E8%BE%BC%E3%82%80)

[Rails で includes して N+1 問題対策 - Qiita](https://qiita.com/hirotakasasaki/items/e0be0b3fd7b0eb350327)

[ActiveRecord::QueryMethods](https://api.rubyonrails.org/classes/ActiveRecord/QueryMethods.html#method-i-includes)

[Rails ActiveRecord/SQL 小技集 - Qiita](https://qiita.com/tomomomo1217/items/1ccd21bfb97730763255#%E5%AD%90%E3%83%A2%E3%83%87%E3%83%AB%E5%AD%AB%E3%83%A2%E3%83%87%E3%83%AB%E3%81%8C%E5%AD%98%E5%9C%A8%E3%81%99%E3%82%8B%E3%83%AC%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E6%8A%BD%E5%87%BA)

## カスタムエラークラスの作成方法

使用用途としては、raiseしたときに、適切なエラーハンドリングをしたい場合は、独自のエラークラスを定義してエラーハンドリングをした方が良い。lib/exceptions.rbを作成して以下のようにする。


```rb
module ApplicationName
  # StandardErrorを継承させる
  class InvalidUserError < StandardError; end
end
```

エラークラスファイルを読み込まなくてはいけないので、config/initializers/exceptions.rbでrequireする

```rb
require 'application_name/exceptions'
```

あとはraiseするときにクラスを指定するだけ

```rb
raise ApplicationName::InvalidUserError
```

## Basic認証の掛け方

```rb
  config.middleware.use Rack::Auth::Basic do |username, password|
    ActiveSupport::SecurityUtils.secure_compare(username, ENV["BASIC_AUTH_USERNAME"]) &
      ActiveSupport::SecurityUtils.secure_compare(password, ENV["BASIC_AUTH_PASSWORD"])
  end
```

このセキュリティ文章は面白い

https://blog.ssrf.in/post/about-rack-utils-secure_compare/

## ActiveReocrdの従属レコードに対してデフォルトでscopeを設定する

Userモデルに従属しているAddressモデルを例に出す。

```
# user.addressesで必ずpost_codeで昇順ソートされる
has_many :addresses, -> { order(post_code: :asc) }, dependent: :destroy
```

## Faradayのエラーレスポンス

初期ではHTTPステータスにより例外が上がらないので、上がるように設定を変える

https://gist.github.com/YuheiNakasaka/2779551#%E3%83%9F%E3%83%89%E3%83%AB%E3%82%A6%E3%82%A7%E3%82%A2%E3%82%92%E4%BD%BF%E3%81%86

http://nekorails.hatenablog.com/entry/2018/09/28/152745#038-FaradayResponseRaiseError---%E7%89%B9%E5%AE%9A%E3%81%AE%E3%82%B9%E3%83%86%E3%83%BC%E3%82%BF%E3%82%B9%E3%82%B3%E3%83%BC%E3%83%89%E3%81%A7%E4%BE%8B%E5%A4%96%E3%82%92%E6%8A%95%E3%81%92%E3%82%8B

https://qiita.com/dany1468/items/2d5e18dee84225ede77d

## Docker起動時のbinding.pryを用いたデバッグ

適当な箇所に対して `binding.pry` を挟んでおく。


```sh
# imageのnameを調べる
docker ps
# attachしてrails serverしているプロセスを表示する
docker attach [image name]

```

これでbinding.pryで止まっているので色々デバッグする

## base64エンコードデータを受け取った時のパース

https://python5.com/q/mtkfaoir

お手軽でこんな感じで実装した

```rb
Base64.strict_decode64(file['data:text/csv;base64,'.length..-1])
```

## Railsコンテナがmysqlクライアントが見つからなくて壊れたら

https://mozy-ok.hatenablog.com/entry/2019/07/23/173210

```
docker-compose run --rm hoge bin/bundle exec gem uninstall mysql2
docker-compose run --rm web bin/bundle install --no-cache
```
