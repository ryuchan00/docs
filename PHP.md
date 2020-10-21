# クレジットカードナンバーのバリデーション

今回PHP5系でやらなきゃならなかったので以下のライブラリを使用した。PHP7系であればもうちょっと選択肢がある。

https://github.com/inacho/php-credit-card-validator

全然Usageに使い方が載っていないのでメモ

composer.json

```
"require": {
    "inacho/php-credit-card-validator": "1.*"
},

```

```php
use Inacho\CreditCard,

// usageに記載のある方法、masterのブランドであれば以下のように返ってくる。しかしmasterでなければキーだけで値が空文字のものが返ってくる。
$card = CreditCard::validCreditCard('5500005555555559', 'mastercard');
print_r($card);

//Array
//(
//    [valid] => 1
//    [number] => 5500005555555559
//    [type] => mastercard
//)

// ブランドがわからない場合は第二引数は入れなくても良い
$card = CreditCard::validCreditCard('5500005555555559');
print_r($card);

//Array
//(
//    [valid] => 1
//    [number] => 5500005555555559
//    [type] => mastercard
//)
```

第一引数は数値でも文字列でも問題はない。

## 正規表現で数値以外を削除する

```php
preg_replace('/[^0-9]/', '', $string);
```

`[^0-9]` で半角数字以外を指定している。したがって全角にする場合は工夫が必要。
