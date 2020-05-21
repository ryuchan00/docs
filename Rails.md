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

```
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

```rails
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
