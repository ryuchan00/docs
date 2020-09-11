## 変更がないことをテストする

資料を参考に一部を改変したコード、User#hoge!実行時に該当のユーザーのrank_idが変わっていないことを確認する。

```rb
describe User do
  describe '#hoge!' do
    let!(:user) { FactoryGirl.create(:user, rank_id: 2) }
    it do
      Rank.stub(:get_rank_id).and_return(2)
      expect{user.hoge!}.not_to change{User.find(user.id).rank_id}
    end
  end
end
```

[メモ：rspecで変更が無いことをテストする - 混沌とした備忘録](https://noboru.hatenablog.jp/entry/2014/02/13/112817)

## FactoryBotを用いて複数インスタンスを生成する create_list

create_listは複数インスタンスを生成してくれる。複数レコードのテストデータが必要なときに便利なやつ。

```rb
  users = create_list(:user, 5, rank: 'diamond')
```

[RSpecでFactoryBotから複数のインスタンスをまとめて作成する【create_listを使用】 - Qiita](https://qiita.com/kodai_0122/items/e755a128f1dade3f53c6)

## 同じテストパターンを使い回す

`shared_examples_for` を使用する。

```rb
# 定義する
shared_examples_for 'ユーザーが複数あるとき' do
  before do
    users.push({name: 'hoge'})
  end

  it 'ユーザーが複数保存される' do
    is_expected.to change(User, :count).by(2)
  end
end

# 実際に呼び出す
context '会員登録が初めての時' do
  before do
    system.status = 'ready'
  end
  
  it_behaves_like 'ユーザーが複数保存される'
end
```

## JSONの一部とマッチングさせる

[r7kamura/rspec-json_matcher: RSpec matcher for testing JSON string](https://github.com/r7kamura/rspec-json_matcher) を使用する

## テストするメソッドを短縮する

`subject` を使用する。使用するには `is_expected` を使用して下記の例のように使用する。

```rb
subject { -> { post "/api/users", params: {name: 'hoge'} }

specify do
  is_expected.to change(User, :count).from(0).to(1)
end
```

## ブロックの中でレコードの数が変化したことを確認する

`change by` `change form to` を使用する。

```rb
specify do
  # Userのレコードが1件増えたことを確認する
  expect{ post `/users`, params: prams}.to change(User, :count).by(1)
end

specify do
  # Userのレコードが0件から1件に変わったことを確認する
  expect{ post `/users`, params: prams}.to change(User, :count).from(0).to(1)
end
```

## インスタンス化されていないインスタンスメソッドの呼び出しを確認する

使用用途として、requestスペックでモデルのインスタンスメソッドの呼び出しを確認したり、モックしたりすることが考えられる。

```rb
before do
  # 必ず呼ばれることを期待するときはexpect_any_instance_ofを使用する
  # この場合User#other_apiが呼ばれないとテストが落ちる
  expect_any_instance_of(User).to receive(:other_api).and_return(true)
end

before do
  # 必ず呼ばれなくても良いが、呼ばれた場合にはモックレスポンスを返してあげたい場合はallow_any_instance_ofを使用する
  allow_any_instance_of(User).to receive(:other_api).and_return(true)
end
```

[使えるRSpec入門・その3「ゼロからわかるモック（mock）を使ったテストの書き方」 - Qiita](https://qiita.com/jnchito/items/640f17e124ab263a54dd)

## Jsonの中身を確認するテストを書く

rspec-json_matcherを使用する

使い方

https://github.com/r7kamura/rspec-json_matcher#usage

良い記事

https://qiita.com/ma2shita/items/a75e43512b43cde712e6

