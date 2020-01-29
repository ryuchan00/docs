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
