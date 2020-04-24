# TypeScript

Web画面で試せる
https://www.typescriptlang.org/play/

## get

get/set アクセサー

[Classes · TypeScript](https://www.typescriptlang.org/docs/handbook/classes.html#accessors)

これはダメな例、fullNmaeにアクセスすることができない。

```typescript
class Employee {
    fullName: string;
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```

こっちは良い例。fullNameの取得と変更ができる。変数のprefixのunderscoreはprivate変数のことを指している。

```typescript
const fullNameMaxLength = 10;

class Employee {
    private _fullName: string;

    get fullName(): string {
        return this._fullName;
    }

    set fullName(newName: string) {
        if (newName && newName.length > fullNameMaxLength) {
            throw new Error("fullName has a max length of " + fullNameMaxLength);
        }
        
        this._fullName = newName;
    }
}

let employee = new Employee();
employee.fullName = "Bob Smith";
if (employee.fullName) {
    console.log(employee.fullName);
}
```

## タイトルバーにプロジェクトルートからのパスを表示させる

settings.jsonを以下のように編集する

```json
"window.title": "${dirty}${activeEditorMedium}${separator}${rootName}",
```

[VSCodeでタイトルにパスを表示してわかりやすくする - Qiita](https://qiita.com/akisx/items/08313fd5b47b5a45bb2f)

## ソフトラップ状態で表示する

settings.jsonを以下のように編集する

```json
"editor.wordWrap": "on" 
```

[How can I switch word wrap on and off in Visual Studio Code? - Stack Overflow](https://stackoverflow.com/questions/31025502/how-can-i-switch-word-wrap-on-and-off-in-visual-studio-code)

## 保存時に行末尾の空白を自動で消す

settings.jsonを以下のように編集する

```json
"files.trimTrailingWhitespace": true,
```

[Visual Studio Codeで保存時に行末尾の空白を自動で消す - Qiita](https://qiita.com/iwata-n/items/39dc0e4391277589878b)

## 配列内の連想配列の型

```typescript
interface Shop {
    id: number,
    name: staring
}

interface Customer {
    id: number,
    shops: Array<Shop>;
}
```

[TypeScript - TypeScriptの配列内の連想配列の型づけ｜teratail](https://teratail.com/questions/166229)
