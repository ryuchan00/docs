## 文字の置き換え

```js
let str = 'abca';
// これだと最初のaのみしか置換されない
str.replace('a','d'); // dbca

str = 'abca';
// 全て置換するためには正規表現を用いる
str.replace(/a/g,'d'); // dbcd
```
