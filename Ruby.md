## メソッドのアスタリスク

本当によく忘れるのでメモ

呼び出す側につける場合は配列、すると配列が引数にバラされる。

```ruby
def hoge(a, b, c, d)
    p a
    p b
    p c
    p d
end
 
arr = [111, 222, 333]
hoge(1, *arr)
```

呼び出される側につけると、配列として受け取る

```ruby
def hoge(a, b, c, *d)
    p a
    p b
    p c
    p d
    p '-----'
end
 
hoge(1, 2, 3, 4, 5)
hoge(1, 2, 3, 4)
hoge(1, 2, 3)
```

[Ruby :: メソッド引数のアスタリスクの意味 [Tipsというかメモ]](http://tm.root-n.com/programming:ruby:etc:parameter_asterisk)

## Rangeオブジェクト

1から100を繰り返すには以下のように書く

```ruby
(1..100).each do {|i| p i}
```

## RubyGemsへのデプロイ

Gemをデプロイしたいけど方法を忘れていたのでメモ

```bash
# RubyGemsへログインする
curl -u [username] https://rubygems.org/api/v1/api_key.yaml > ~/.gem/credentials; chmod 0600 ~/.gem/credentials 
rake build
rake release
```

## with_index

`map` と組み合わせることによって、ハッシュが要素の配列が簡単に作れたりする

```ruby
hoge.map.with_index do |v,k|
    {value: v,key: k}
end
# [{value: v,key: k},{value: v,key: k}...]
```

[Enumerator#with_index (Ruby 2.7.0 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Enumerator/i/with_index.html)

## ハッシュ値を生成する

```ruby
require 'digest'

plain_text = 'password'

puts 'MD5:    ' + Digest::MD5.hexdigest(plain_text)
puts 'RMD160: ' + Digest::RMD160.hexdigest(plain_text)
puts 'SHA1:   ' + Digest::SHA1.hexdigest(plain_text)
puts 'SHA256: ' + Digest::SHA256.hexdigest(plain_text)
puts 'SHA384: ' + Digest::SHA384.hexdigest(plain_text)
puts 'SHA512: ' + Digest::SHA512.hexdigest(plain_text)

```

[【Ruby】文字列のハッシュ値を取得(SHA, MD5, RMD160) - Qiita](https://qiita.com/kou_pg_0131/items/ad7a0fdd7b0a997cd5f4)

詳しい仕様は、Rubyのlibrary digestを参照する。

[library digest (Ruby 2.7.0 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/library/digest.html)

## モジュールをインクルード時に実行するメソッド

`included` を使用する。

```rb
module Foo
  def self.included(mod)
    p "#{mod} include #{self}"
  end
end
class Bar
  include Foo
end
# => "Bar include Foo"
```

[Module#included (Ruby 2.7.0 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Module/i/included.html)

## Hashのキーから割り出す

```rb
h = {1=>"a", 2=>"b", 3=>"c"}

p h.values_at(1,3,4)               #=> ["a", "c", nil]
# [h[1], h[3] ,h[4]] と同じ
```

[Hash#values_at (Ruby 2.7.0 リファレンスマニュアル)](https://docs.ruby-lang.org/ja/latest/method/Hash/i/values_at.html)

## mysql2のGemのインストールに失敗する

mysql2のGemを含むGemfileをbundle installしていたら次のようなエラーが出た。

```log
❯ bundle installa
Fetching gem metadata from https://rubygems.org/......
Using rake 12.3.3
Using concurrent-ruby 1.1.5
Using i18n 0.9.5
Using minitest 5.12.0
Using thread_safe 0.3.6
Using tzinfo 1.2.5
Using activesupport 4.2.11.1
Using builder 3.2.3
Using activemodel 4.2.11.1
Using arel 6.0.4
Using activerecord 4.2.11.1
Using bcrypt_pbkdf 1.0.1
Using bundler 1.17.3
Using coderay 1.1.2
Using composite_primary_keys 8.1.7
Using enumerize 2.3.1
Using factory_girl 4.9.0
Fetching mysql2 0.3.18
Installing mysql2 0.3.18 with native extensions
Gem::Ext::BuildError: ERROR: Failed to build gem native extension.

    current directory: /Users/yamakawa00/.rbenv/versions/2.5.1/lib/ruby/gems/2.5.0/gems/mysql2-0.3.18/ext/mysql2
/Users/yamakawa00/.rbenv/versions/2.5.1/bin/ruby -r ./siteconf20191101-23378-6o0uiy.rb extconf.rb
--with-ldflags=-L/usr/local/opt/openssl/lib --with-cppflags=-I/usr/local/opt/openssl/include
checking for ruby/thread.h... yes
checking for rb_thread_call_without_gvl() in ruby/thread.h... yes
checking for rb_thread_blocking_region()... no
checking for rb_wait_for_single_fd()... yes
checking for rb_hash_dup()... yes
checking for rb_intern3()... yes
-----
Using mysql_config at /usr/local/opt/mysql@5.6/bin/mysql_config
-----
checking for mysql.h... yes
checking for errmsg.h... yes
checking for mysqld_error.h... yes
-----
Don't know how to set rpath on your system, if MySQL libraries are not in path mysql2 may not load
-----
-----
Setting libpath to /usr/local/opt/mysql@5.6/lib
-----
creating Makefile

current directory: /Users/yamakawa00/.rbenv/versions/2.5.1/lib/ruby/gems/2.5.0/gems/mysql2-0.3.18/ext/mysql2
make "DESTDIR=" clean

current directory: /Users/yamakawa00/.rbenv/versions/2.5.1/lib/ruby/gems/2.5.0/gems/mysql2-0.3.18/ext/mysql2
make "DESTDIR="
compiling client.c
compiling infile.c
compiling mysql2_ext.c
compiling result.c
linking shared-object mysql2/mysql2.bundle
ld: library not found for -limported_openssl
clang: error: linker command failed with exit code 1 (use -v to see invocation)
make: *** [mysql2.bundle] Error 1

make failed, exit code 2

Gem files will remain installed in /Users/yamakawa00/.rbenv/versions/2.5.1/lib/ruby/gems/2.5.0/gems/mysql2-0.3.18 for
inspection.
Results logged to
/Users/yamakawa00/.rbenv/versions/2.5.1/lib/ruby/gems/2.5.0/extensions/x86_64-darwin-18/2.5.0-static/mysql2-0.3.18/gem_make.out

An error occurred while installing mysql2 (0.3.18), and Bundler cannot continue.
Make sure that `gem install mysql2 -v '0.3.18' --source 'https://rubygems.org/'` succeeds before bundling.

In Gemfile:
  colorme-fixtures was resolved to 0.0.1, which depends on
    mysql2
```

色々試して、mysqlコマンドが実行できない、Opensslは入ってるので、もしかしてmysql2のバージョンが現在入っているOpensslのバージョンに対応していないのでは？と思ったので試してみることにした。そしたらうまくいった。

```log
brew install openssl@1.0.2r
brew switch openssl 1.0.2t
mysql.server start
```

[MacでMySQL起動時にエラー(dyld: Library not loaded: /usr/local/opt/openssl/lib/libssl.1.0.0.dylib) - Qiita](https://qiita.com/hirotech/items/d16988b79f7673fa2d31)

## Ruby で hash の配列から、特定の key の value のみの配列を作成する

よく忘れるやつ

```rb
hoge = [{a: 1, b: 2}, {a: 3, b: 4}]
hoge.map{|h| h[:a]}  # [1,3]
```

[Ruby で hash の配列から、特定の key の value のみの配列を作成する - Qiita](https://qiita.com/longtime1116/items/21f706d37606c2ceeec1)

## RubyGemsがデプロイできなくなった

```sh
active_merchant-epsilon 0.11.0 built to pkg/active_merchant-epsilon-0.11.0.gem.                                                                             (31/490 results) [2497/15241]
Tagged v0.11.0.                                                                                                                                                                          
Untagging v0.11.0 due to error.                                                                                                                                                          
rake aborted!                                                                                                                                                                            
Couldn't git push. `git push ' failed with the following output:                                                                                                                         
                                                                                                                                                                                         
remote: Permission to pepabo/active_merchant-epsilon.git denied to pepabot.                                                                                                              
fatal: unable to access 'https://github.com/pepabo/active_merchant-epsilon.git/': The requested URL returned error: 403                                                                  
                                                                                                                                                                                         
/Users/yamakawa00/.rbenv/versions/2.6.5/bin/bundle:23:in `load'                                                                                                                          
/Users/yamakawa00/.rbenv/versions/2.6.5/bin/bundle:23:in `<main>'                                                                                                                       
Tasks: TOP => release => release:source_control_push                                                                                                                                     (See full trace by running task with --trace)               
```

いくつもの原因が重なった面倒な奴だった。GitHubにはhttpを使用している。まずMacのkeychainに入れたはずのGitHubへのPersonal Access Tokenが間違っていた。何かの拍子に書き換えてしまったようだ。とりあえず削除して、改めてリリースコマンドを叩いてPersonal Access Tokenを入力してみると、RubyGemsへの権限がないようだ。以前はできたけど、何かの拍子でcredential情報を消してしまったようだったので、改めて作り直したらいけた。あまり再現性はないが記録を残しておく。
