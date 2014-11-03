Rails
---

基本操作
---

* rails new [app]  
   Appのひな形作成  
   `rails new [app] --database=mysql --skip-bundle`  
   DBをmysqlにbundle installをスキップしてくれる
* rails generate scaffold page name:string title:string  
   controllerとmodelを作成してくれる  
* rails g scaffold_controller モデル名
  既存のモデルのでcontrollerとviewを作成してくれる
* rails generate model page name:string title:string  
   modelの作成  
   db/migrate/日付_create_page.rbができる  
* rails generate controller Page  
   controllerの作成  
   `rails generate controller page title`  
   controllerとviewの作成  
   `rails generate controller 'admin/page'`  
   階層化された  
* rails console  
   コンソールの起動  
* rails db  
   DBへのアクセス  
* rails server  
   サーバへアクセスする
   rails s -e production
   本番環境で構築

## migration

rails generate migration クラス名 カラム名:データ型( カラム名:データ型)
```
# model のデータを作成する時
rails generate model group name:string describe:string 

# リレーション（外部キー設定）
rails generate model user_group user_id:integer:index group_id:integer:index

# ユニークキー設定
rails generate migration add_index_to_table_name column_name:uniq
```

```
def change
   # 今回の変更で実行する内容
end

def self.up
   # ロールバックできない処理をする際に使用する
end

def self.down
   # 
end
```

### create_table

### drop_table

### rake command

* db:migrate runs (single) migrations that have not run yet.
* db:create creates the database
* db:drop deletes the database
* db:schema:load creates tables and columns within the (existing) database following schema.rb
* db:setup does db:create, db:schema:load, db:seed
* db:reset does db:drop, db:setup

* db:create creates the database for the current env
* db:create:all creates the databases for all envs
* db:drop drops the database for the current env
* db:drop:all drops the databases for all envs
* db:migrate runs migrations for the current env that have not run yet
* db:migrate:up runs one specific migration
* db:migrate:down rolls back one specific migration
* db:migrate:status shows current migration status
* db:rollback rolls back the last migration
* db:forward advances the current schema version to the next one
* db:seed (only) runs the db/seed.rb file
* db:schema:load loads the schema into the current env's database
* db:schema:dump dumps the current env's schema (and seems to create the db as well)
* db:setup runs db:schema:load, db:seed
* db:reset runs db:drop db:setup
* db:migrate:redo runs (db:migrate:down db:migrate:up) or (db:rollback db:migrate:migrate) depending on the specified migration
* db:migrate:reset runs db:drop db:create db:migrate

## log

## Controller

### ログの書き方
logger.debug

### エラーページ

`Rails.application.config.consider_all_requests_local`は、開発環境でのアクセスという設定である。
[purpose-of-consider-all-requests-local-in-config-environments-development-rb](http://stackoverflow.com/questions/373089/purpose-of-consider-all-requests-local-in-config-environments-development-rb)

`config/environments/development.rb`で設定されている。

config/routes.rb

```
 ...
 unless Rails.application.config.consider_all_requests_local
   match '*not_found', to: 'application#render_404' 
 end
end
```

app/controllers/application_controller.rb

```
unless Rails.application.config.consider_all_requests_local 
   rescue_from Exception,                           with: :render_500
   rescue_from ActionController::RoutingError,      with: :render_404
end

```


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

### Capistrano

```
# productionのデプロイ
cap production deploy
# 一つ前のバージョンに戻す
cap deploy:rollback
# migrate
cap deploy:migrate
# updatecode & migrate
cap deploy:migrations
# 古いやつを削除　default 5つ
cap deploy:cleanup
```

### Jenkins


Gemは `ci_reporter`を利用する

https://github.com/ci-reporter/ci_reporter

```
group :test do
  gem 'ci_reporter'
end

```

lib/lib/tasks/ci_reporter.rake　を作成する

```
if ENV['GENERATE_REPORTS'] == 'true'
    require 'ci/reporter/rake/rspec'
    task :spec => 'ci:setup:rspec'
end
```

Rakefileに設定する

```
require 'ci/reporter/rake/rspec'
```

Jenkins側で bundle exec rake ci:setup:rspec spec を実行する


