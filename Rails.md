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
