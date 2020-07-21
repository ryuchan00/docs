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
# Angularでcookieを処理する時に使用するAngularngx-cookie-service を使用する
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

[ngx-cookie-service - npm](https://www.npmjs.com/package/ngx-cookie-service)

[Angular 9— How to use cookies - ITNEXT](https://itnext.io/angular-8-how-to-use-cookies-14ab3f2e93fc)

## HttpClientXsrfModuleが動かない

CSRFの回避のためにCSRFトークンをクッキーからHTTPヘッダーに付与しようとしたが、HTTPヘッダーにCSRFトークンがなかった。原因はHTTPリクエストを絶対パスで行なっているためである。相対パスで行なった場合のみHttpClientXsrfModuleが機能する。これは、同一のドメインでないとダメということを暗黙的に表現しているのだろうか？

[HttpClientXsrfModuleで設定したヘッダがリクエストに反映されない - トラブルシューティング - Discourse | Angular Japan User Group](https://angular-japan.discourse.group/t/topic/105)

## Angularのlintの方法

```sh
# lintを走らせて、書き方がエラーの部分を見つけるだけ
# これを実行するとng lintが走るようになっている
npm run lint

# 自動で修正する
npm run lint -- --fix
```

## ng generate時のmodule.tsが存在する場合のmoduleの指定

例としてapp.module.tsとapi.module.tsの二つがあるとする。そうするとどちらのmoduleに編集を加えればいいかわからないので、 `--module` オプションを使用する。

```sh
# app.module.tsに対して、編集しつつ、userコンポーネントを作成する
ng g component user --module app 
```

## 外部APIを使用した時のcomponent.htmlでの遅延回避方法

下記のコードを実行した場合、userに外部APIのレスポンスが入る前にhtmlがロードされてしまい、consoleにエラーが出てしまう。

user.component.ts

```ts
ngOnInit(): void {
  this.userService.getUsers.subscccribe((data) => {
    this.user = data;
  });
}
```

```html
{{user.name}}
```

回避方法は、?をつけてuserが存在しない場合なnameメソッドを使用しないことである。

```html
{{user?.name}}
```

## パスのidを取得する方法

`/users/1` のようにidがurlに入った場合に、それを取得するためにはどうすれば良いか。 `ActivatedRoute` モジュールを使用するのが良い。

ルーティングの設定を app-routing.module.tsで行う。

```ts
const routes: Routes = [
  { path: 'users/:id, component: UserComponent },
];
```

user.component.tsは以下のように編集する。

```ts
import { ActivatedRoute } from '@angular/router';

export class UserComponent implements OnInit {
  constructor(
    private route: ActivatedRoute,
  ) {}
  
  ngOnInit(): void {
    // idを取得する、+はjsの構文で、文字列を数値に変換する
    console.log(+this.route.snapshot.paramMap.get('id'));
  }
}
```

[Angular 日本語ドキュメンテーション - ルーティングを使ったアプリ内ナビゲーションの追加](https://angular.jp/tutorial/toh-pt5#%E3%83%AB%E3%83%BC%E3%83%86%E3%82%A3%E3%83%B3%E3%82%B0%E5%8F%AF%E8%83%BD%E3%81%AA-herodetailcomponent)


## フォームの作成方法

## Angular Matrialで
