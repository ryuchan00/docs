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

## with_index

`map` と組み合わせることによって、ハッシュが要素の配列が簡単に作れたりする

```ruby
hoge.map.with_index do |v,k|
    {value: v,key: k}
end
# [{value: v,key: k},{value: v,key: k}...]
```

[Enumerator#with_index (Ruby 2.7.0 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Enumerator/i/with_index.html)

## ハッシュ値を生成する

```ruby
require 'digest'

plain_text = 'password'

puts 'MD5:    ' + Digest::MD5.hexdigest(plain_text)
puts 'RMD160: ' + Digest::RMD160.hexdigest(plain_text)
puts 'SHA1:   ' + Digest::SHA1.hexdigest(plain_text)
puts 'SHA256: ' + Digest::SHA256.hexdigest(plain_text)
puts 'SHA384: ' + Digest::SHA384.hexdigest(plain_text)
puts 'SHA512: ' + Digest::SHA512.hexdigest(plain_text)

```

[【Ruby】文字列のハッシュ値を取得(SHA, MD5, RMD160) - Qiita](https://qiita.com/kou_pg_0131/items/ad7a0fdd7b0a997cd5f4)

詳しい仕様は、Rubyのlibrary digestを参照する。

[library digest (Ruby 2.7.0 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/library/digest.html)
