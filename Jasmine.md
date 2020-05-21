## 部分的にテストを実行する

focusを使用する

```
  fit("is focused and will run", function() {
    expect(true).toBeTruthy();
  });

	
  fdescribe('focused describe', function(){
    it('will run', function(){
      expect(true).toBeTruthy();
    });
```

[focused_specs.js](https://jasmine.github.io/2.1/focused_specs.html)

## テストの実行をスキップする

pendingまたは `x` をつける

```
  xit("can be declared 'xit'", function() {
    expect(true).toBe(false);
  });
  
  it("can be declared by calling 'pending' in the spec body", function() {
    expect(true).toBe(false);
    pending();
  });

```

[introduction.js](https://jasmine.github.io/2.0/introduction#section-Pending_Specs)

## メソッドが呼び出されているかテストする

```ts
beforeEach() => {
	spyOn("テストしたいオブジェクト", '"テストしたいメソッドの文字列"');
	user.register;  // テストしたいメソッドが呼び出されている処理をここで実行してしまう
}));
it("should call", () => {
	// validメソッドが呼び出されているか
	expect(user.valid).toHaveBeenCalled();
});
```

[JasmineのSpy機能でテストダブルを作成する - Build Insider](https://www.buildinsider.net/web/bookjslib111/109)
