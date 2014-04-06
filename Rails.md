## log

## Controller

ログの書き方
logger.debug


## Guard

rb-fbeventはlinuxにはインストールできない

tipsはspring rspecで高速化を図っているため
spring rspecができるかチェックする
spring は bundle install ではなくgem installで動かすのがいい
spring はデフォルトでspring 

~/.spring.rb
require 'spring-commands-rspec'

## Rspec

## 基本

#### nilかどうか
be_nil

## モック　&&スタブ

Stub

```
[Object].stub(:[Method]).and_return([戻り値])
```

すべてのインスタンスメソッドに適応させたい場合は、any_instanceを利用する
だだし、rspec が　2.14以前である
```
[Object].any_instance.stub(:[Method]).and_return([戻り値]) 
GollumWiki.any_instance.stub(:init_repo).and_return(false)
```

Mock

```
[Object].should_recieve(:[Method]).with([引数]).and_return([戻り値])
```

before はdefaultは:eachである
基本的に同じスコープ内であれば実行される

## shared_example_for

テストのテンプレート化ができる

shared_examples_for 'test' do
end
it_behavies_like('test')

### Modelのテストについて

subject がモデルである

## rabl

テンプレートでJSONの内容を指定できる
複雑なJSON時に便利になる

.rabl

## Grape

簡単にAPIが作成できる

```ruby

mount API_wewe

```

ログ
API.logger.info


### Productionでの動かし方

#### cssが読み込めない
`config.assets.compile = true`にする

http://stackoverflow.com/questions/15206789/rails-not-serving-assets-in-production-or-staging-environments

#### Unicorn起動
`unicorn_rails -c config/unicorn.rb -E production -D`
`ps -ef | grep unicorn | grep -v grep`


