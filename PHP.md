# composerの使用方法

`composer install` はcomposer.lockファイルがないときにlockファイルを作成して、ライブラリを `./vendor/` に置く。

`composer update` はcomposer.jsonに追加してときにlockファイルに書き込んでくれる。ただupdateするだけだと、jsonに書かれた全てのパッケージを更新するので、場合によっては `composer update [パッケージネーム]` のように指定する必要もある。

普段使用しているbundlerと仕様が違って、久しぶりに使用するとあれ？となった。癖でinstallしてもlockが更新されない。。。となった。

[Composerの導入＆使い方（初心者の復習用） - Qiita](https://qiita.com/suke/items/770bccf8a43f9247daf5)

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

## array_searchを用いてarrayをマッチングキーにする

```php
// ループの中での話
// $settings = array(array('a','b'),array('c','d'));
// と定義されていたら、$settings_keyがarray('a','b')はマッチする。

$key = array_search($settings_key, $settings);
if ($key === false) {
    // アンマッチ
}

$settings_key[] = array(
    $number,
    $title]
);
```

[PHP: array_search - Manual](https://www.php.net/manual/ja/function.array-search.php)
