# TypeScript

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
