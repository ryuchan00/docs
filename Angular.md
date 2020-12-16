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

### フラッシュメッセージの出し方

snackBarを使用する。コンポーネントを作成しない場合は以下のような感じにする。

```js
this.snackBar.open('msg', 'close', {
                  duration: 2000
                }).afterDismissed().subscribe(()=>{
                  this.router.navigate(['']);
                });
```

[Angular show snackbar after navigation - Stack Overflow](https://stackoverflow.com/questions/56459195/angular-show-snackbar-after-navigation)


### mat-selectタグで変更を検知したい

通常の `(chage)=` みたいなHTMLelemtntのchageイベントは使用できなかった。 `selectionChange`を使用する。

```
<mat-select formControlName="inputType" (selectionChange)="OnInputTypeChange(optionField)">
```

[Select | Angular Material](https://material.angular.io/components/select/api)

stackoverflowの回答よりヒントを得た。

[typescript - How can I get new selection in "select" in Angular 2? - Stack Overflow](https://stackoverflow.com/questions/33700266/how-can-i-get-new-selection-in-select-in-angular-2)

### フォームのエラーメッセージ

[Form field | Angular Material](https://material.angular.io/components/form-field/overview#error-messages)

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

以下のような回避方法もある。

[Using Angular Forms with Async Data - Angular 10 | 9](https://coryrylan.com/blog/using-angular-forms-with-async-data)

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

## Angular Matrialでリストを作成する

ユーザ一覧のように一覧画面を作成する。ユーザの名前をクリックしたら遷移させたい。ユーザにマウスホバーしたら色を変えたい。

<mat-nav-list>タグを使う。

```html
<mat-nav-list>
  <mat-list-item *ngFor="let user of users">
    <!-- matLineで幅いっぱいの線を引く -->
    <a matLine href="./users/{{ user.id }}">{{ user.name }}</a>
    <!-- 下線を引く -->
    <mat-divider></mat-divider>
  </mat-list-item>
</mat-nav-list>
```

[List | Angular Material](https://material.angular.io/components/list/overview#navigation-lists)

## FormArray内のデータを編集する

getでキーを指定して、setValueで値を代入する。

```ts
users.controls[i].get('name').setValue('');
```

[Angular の FormArray で項目数が動的に増える入力フォームを実現する - Corredor](https://neos21.hatenablog.com/entry/2018/05/21/080000)

## @Inputデコレータ

htmlからコンポーネントにデータを渡したい！という時に使用する。

完成イメージ

```html
<employee lastname="Yamada" firstname="Hanako" title="HR"></employee>
```

component.ts

```ts
import {Component, Input} from '@angular/core';

@Component({
  selector: 'employee',
  templateUrl: './employee.component.html',
  styleUrls: [ './employee.component.css']
})
export class EmployeeComponent{
  @Input() lastname;
  @Input() firstname;
  @Input() title;
}
```

component.html

```html
<div class='emp'>
  <div class='emp-name'>{{lastname|uppercase}}, {{firstname}}</div>
  <div class='emp-title'>{{title}}</div>
</div>
```

[入力プロパティ @Input() - Angular の基礎 - Angular 入門](https://angular.keicode.com/basics/template-input-properties.php)

## @NgModuleデコレーター

これがあるとAppModuleクラスであることを表す。

### declarations

AppModuleクラスの中に@NgModuleでこれー他は存在する。declarationは、Angularアプリケーションに含まれるクラスはどれなのか指定している。

## Routerモジュールをテストするとき

`RouterTestingModule` をインポートする。

[Angular - RouterTestingModule](https://angular.io/api/router/testing/RouterTestingModule)

## 親のコンポーネントに対してマウスイベントの伝搬を停止する

`click` イベントが親のコンポーネントで書いた `click` イベントも一緒に発火してしまった。おそらくコンポーネントの問題ではなく、JavaScriptの問題であるが、Angularアプリケーションの作成中に遭遇したものなので、こちらに記載しておく。以下が実際に遭遇した例である。

```
<mat-icon aria-hidden="false" (click)="onDeleteUserClick; $event.stopPropagation()">delete</mat-icon>
```

対策としてはclickイベント発火後に、 `$event.stopPropagation()` を実行してあげると、イベントの伝搬がストップした。

[angular — マウスイベントの伝播を停止する](https://www.it-swarm.dev/ja/angular/%e3%83%9e%e3%82%a6%e3%82%b9%e3%82%a4%e3%83%99%e3%83%b3%e3%83%88%e3%81%ae%e4%bc%9d%e6%92%ad%e3%82%92%e5%81%9c%e6%ad%a2%e3%81%99%e3%82%8b/824110400/)

その他とMDN web docsを見てみると、ドキュメントを見つけた。 `stopPropagation()` は現在のイベントのキャプチャリングまたはバブリングの過程における伝搬を抑止する。

[Event.stopPropagation() - Web API | MDN](https://developer.mozilla.org/ja/docs/Web/API/Event/stopPropagation)

バブリングとキャプチャリングの情報をこちらを参照

バブリング

> 要素上でイベントが起きると、最初にその上のハンドラが実行され、次にその親のハンドラが実行され、他の祖先に到達するまでそれらが行われます。

[バブリング と キャプチャリング](https://ja.javascript.info/bubbling-and-capturing)

## フォームのバリデーション

[Angular 日本語ドキュメンテーション - Validating form input](https://angular.jp/guide/form-validation)

## ngIfやngForのためだけにdivを設置するのをやめるng-content

[Angularの便利タグng-container, ng-content, ng-template - Qiita](https://qiita.com/shibukawa/items/c8c7fd22c1054348db3a#ng-content)

## 複数インプットを同時にバリデーションするcross-validation

[Angular 日本語ドキュメンテーション - Validating form input](https://angular.jp/guide/form-validation#adding-cross-validation-to-template-driven-forms)

mat-errorとの組み合わせについて

https://itnext.io/materror-cross-field-validators-in-angular-material-7-97053b2ed0cf

## maxlengthとtype="number"は一緒に使えない

https://stackoverflow.com/questions/18510845/maxlength-ignored-for-input-type-number-in-chrome

## maxlengthのバリデーションの例

https://www.concretepage.com/angular-2/angular-2-4-minlength-and-maxlength-validation-example#maxlength

MaxLngthValidatorを使用する

https://angular.jp/api/forms/MaxLengthValidator

## subscribeのエラー処理

https://programming-jissen.com/how-to-handle-errors-with-angular-subscribe-method/

```js
getError(): void {
  this.heroService.getHeroes()
  .subscribe(
    heroes => {
      console.log('OK！');
      this.heroes = heroes;
    }, error => {
      console.log('エラーですよ');
    });
}
```

## @Opuputデコレータ

双方向データバインディングのために使用する

## Angularのsleep処理

[Angular8のtypescriptでsleep処理を実装する | SaToLABO](https://satolabo.net/2019/12/14/angular-typescript-sleep/)

## base64エンコードしたデータをAPIへ送る

```ts
    const fr = new FileReader();
    fr.onload = (e) => {
      // e.target.resultがbase64エンコードされた文字列
      console.log(e.target.result);
      this.apiClient
        .postData({data: e.target.result})
        .subscribe(
          () => {
            // ファイルのインポートに成功
          },
          () => {
            // ファイルのインポートに失敗
            );
          }
        );
    };
    fr.readAsDataURL(file);  // fileはFileオブジェクト
```
## ローディング処理を実装する

Angular Materialを使用する。

この参考は、app.moduleeee.tsにもimportしているが、必要ない。

[Angular Materialでローディング（処理中）を実装する - Qiita](https://qiita.com/shinoshu/items/9d9480d1499c14d2d6d0)
