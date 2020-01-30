## Directiveのscopeの書き方

```ts
scope: {
  myDirective: '=',
  title: '@',
  onChange: '&'
}
```

> 「プロパティ名：記号」の記法であり、記号の種類として、`'@'`, `'='`, `'&'`の3種類が指定可能である。

> - `'@'` 子Scopeのプロパティ値をディレクティブのHTML属性値と紐付ける。
> `<div my-drirective="my name is {{name}}"></div>`, `scope:{myDirective:'@'}` であれば、このscope.myDirectiveの値は、"my name is ○○"となる(○○は親Scopeにおける`name`の値)
> - `'='` 子Scopeのプロパティを親Scopeのプロパティと双方向バインディングで紐付ける。親Scopeプロパティが変更されれば、子Scopeのプロパティは変更され、子が変更されれば、親も変更される。親scopeの値を使って、このディレクティブの状態を管理したいときに有用。  
> - `'&'` 子ScopeのプロパティにAngularのExpression関数を紐付ける。ディレクティブのイベント発生時に発火するイベントハンドラの設定ポイントをディレクティブ利用者へ提供するときに有用。
> 
> また、'@', '=', '&'記号の後ろにディレクティブ要素中での属性名を明示することで、子scopeのプロパティ名とディレクティブ中での属性名を別名にすることも可能。

引用元

[AngularJSのDirectiveを理解する. - Qiita](https://qiita.com/Quramy/items/dd4e7d2693c32d92048c#scope)
