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
