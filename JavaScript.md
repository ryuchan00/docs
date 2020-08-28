## 文字の置き換え

```js
let str = 'abca';
// これだと最初のaのみしか置換されない
str.replace('a','d'); // dbca

str = 'abca';
// 全て置換するためには正規表現を用いる
str.replace(/a/g,'d'); // dbcd
```
## 要素の削除

filterをかけて新しく配列を作成し直した方が良い

```js
const array = [0, 1, 2, 3, 4];
const newArray = array.filter(n => n !== 1);
// 1だけ削除する
```

[Javascriptで指定した配列の要素を削除する - Qiita](https://qiita.com/Sekky0905/items/598b47fea2106b8c140e#%E8%BF%BD%E8%A8%98)

## indexをつけながら配列を回す

```js
users.forEach((user, index) => {
});
```

[for ... of 文でインデックス(index)の値を使う方法 - Qiita](https://qiita.com/TakahiRoyte/items/dca532dd64bc782ad849)

## リダイレクトの方法

```
location.href = 'https://hoge.jp/users/' + id
```

[javascriptで転送（リダイレクト）を行う方法と注意点 | SEO研究所サクラサクラボ](https://www.sakurasaku-labo.jp/blogs/javascript-redirect)


## とある条件の配列を作成する

下記のイメージされた配列を作成したい

```
data = [{profiles: {name: "graham", age: 27}, {name: "setsuna", age: 16}}];
// これを ["graham", "setsuna"] みたいにしたい
```

```js
// 以下のように書く
Array.from(data.profiles, (profile) => 
  profile.name
)
```

うーん、あんまりイメージ例はよくないかも

## 配列の中でのmax値を取得する`

```
console.log(Math.max(...array1));
// expected output: 3

```

[Math.max() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

`...` はスプレッド構文、端折って説明すると配列を可変長引数にしたりできる。ES2015から使用できる。

[スプレッド構文 - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Operators/Spread_syntax)

## オブジェクトのコピー

JavaScriptは引数は参照渡しになるので、オブジェクトのコピーが必要になる時がある。

[ES6でオブジェクトのコピー - Qiita](https://qiita.com/yamayamasan/items/2afed39e1a34edc44779)

[Object.assign() - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)

lodashの使用

[typescript - Deep copy of Angular Reactive Form? - Stack Overflow](https://stackoverflow.com/questions/48308414/deep-copy-of-angular-reactive-form/49743369)

[Lodash Documentation](https://lodash.com/docs/4.17.15#cloneDeep)

[Angular: Deep copy a reactive form - Stack Overflow](https://stackoverflow.com/questions/57387998/angular-deep-copy-a-reactive-form)

[Using lodash in Angular 4 - Stack Overflow](https://stackoverflow.com/questions/48836140/using-lodash-in-angular-4/48836201)

## 正規表現

[正規表現 - JavaScript | MDN](https://developer.mozilla.org/ja/docs/Web/JavaScript/Guide/Regular_Expressions)

```js
// 数値のみ、1桁から11桁
\d{1,11}
```

