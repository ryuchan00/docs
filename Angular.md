# Angular

## nodenvのインストール

今回brewでインストールされたnode-buildではnodeの最新版が取得できなかったのでnode-buildは手動で入れた。

https://github.com/nodenv/nodenv#installation

https://github.com/nodenv/node-build#installation

```
brew install nodenv
mkdir -p "$(nodenv root)"/plugins
cd ~/.nodenv
cd plugins/node-build
git clone https://github.com/nodenv/node-build.git "$(nodenv root)"/plugins/node-build
git pull
nodenv install --list
nodenv install 12.16.1
nodenv local 12.16.1
```

## AngularMaterial

https://v5.material.angular.io/

## Angularの日本語公式ドキュメント

https://angular.jp/

## Angularチュートリアルを終わらせた人がやるやつ

[イントロダクション - Angular After Tutorial](https://gitbook.lacolaco.net/angular-after-tutorial/season-2-effective-rxjs/introduction)

## Angularでcookieを利用する

今回やったのはcookieの値を取得すること。

```sh
# ngx-cookie-service を使用する
npm install ngx-cookie-service --save
```

app.module.ts

```ts
import { CookieService } from 'ngx-cookie-service';
// ...
  providers: [CookieService ],
// ...
```

利用するコンポーネントでもimportする。僕は今回はIntercptorクラスに追加した。

```ts
import { CookieService } from 'ngx-cookie-service';

export class HogeInterceptor implemnts HttpInterceptor {
  constructor(private cookieService: CookieService) {}
  const token = this.cookieService.get('CSRF-TOKEN');
  console.log(token)  // 'token string....'
}
```

[Angular 5でcookieを利用する - Qiita](https://qiita.com/second113/items/754cf6b63bfe89702dce)
