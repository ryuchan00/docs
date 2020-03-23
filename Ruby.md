## メソッドのアスタリスク

本当によく忘れるのでメモ

呼び出す側につける場合は配列、すると配列が引数にバラされる。

```ruby
def hoge(a, b, c, d)
    p a
    p b
    p c
    p d
end
 
arr = [111, 222, 333]
hoge(1, *arr)
```

呼び出される側につけると、配列として受け取る

```ruby
def hoge(a, b, c, *d)
    p a
    p b
    p c
    p d
    p '-----'
end
 
hoge(1, 2, 3, 4, 5)
hoge(1, 2, 3, 4)
hoge(1, 2, 3)
```

[Ruby :: メソッド引数のアスタリスクの意味 [Tipsというかメモ]](http://tm.root-n.com/programming:ruby:etc:parameter_asterisk)

## Rangeオブジェクト

1から100を繰り返すには以下のように書く

```ruby
(1..100).each do {|i| p i}
```

## RubyGemsへのデプロイ

Gemをデプロイしたいけど方法を忘れていたのでメモ

```bash
# RubyGemsへログインする
curl -u [username] https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials 
rake build
rake release
```
