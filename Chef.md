# Chef

## 公式

docs   
http://docs.opscode.com/  

community   
http://community.opscode.com/  

### Chef Solo 
１台でもChefを利用出来るようにしている。  
そのためにVagrantを利用することが多い。  
Chefを使用する流れとしてはVagrantで立ち上げたVMにChefの`kinfe solo`コマンドでCookBookを転送、VM上でCookBookを実行する。  

## Install

```Bash
$ gem i chef knife-solo test-kitchen serverspec berkshelf foodcritic --no-ri --no-rdoc
```   

## Command

wikiみたいな物
http://matschaffer.github.io/knife-solo/

```Bash
# kitchenを作成する
$ knife solo init DIRECTORY
# HOSTにChefをインストールするかつnodesフォルダに新たに
# HOSTNAME.jsonが追加される
$ knife solo prepare [USER@]HOSTNAME [JSON] (options)
# kitchenをHOSTにアップロードし、chef-soloを実行する
$ knife solo cook [USER@]HOSTNAME [JSON] (options)
# prepareとcookを一度に行う 
$ knife solo bootstrap [USER@]HOSTNAME [JSON] (options)
# アップロードされたkitchenを削除する
$ knife solo clean [USER@]HOSTNAME
# DIRECTORYに新たにCOOKBOOKNAMEが追加される
$ knife cookbook create COOKBOOKNAME -o DIRECTORY
# 構文チェック
$ knife cookbook test COOKBOOKNAME
```

## init  

chefのKitchenとCookbookの作成を行います  

```Bash
# Kitchen
$ knife solo init chef-repo
$ tree .
.
├── Berksfile
├── Berksfile.lock
├── cookbooks　　　　#共通のcookbook berksfileなどで外部から取ってくるcookbook
├── data_bags
├── nodes
├── roles
└── site-cookbooks　　#特定のcookbooksつまり自分が作成したレシピはここに置いたほうがよい 

# CookBook
$ knife cookbook create hello -o ./site-cookbooks
** Creating cookbook hello
** Creating README for cookbook: hello
** Creating CHANGELOG for cookbook: hello
** Creating metadata for cookbook: hello

$ cd ./site-cookbooks/hello/
$ tree .
.
└── hello
    ├── CHANGELOG.md
    ├── README.md
    ├── attributes
    ├── definitions
    ├── files
    │   └── default
    ├── libraries
    ├── metadata.rb
    ├── providers
    ├── recipes
    │   └── default.rb
    ├── resources
    └── templates
        └── default
```

## Node  

```Bash
$ ssh centos #←接続するサーバ  
$ knife solo prepare centos #←接続するサーバ  
```  

するとnodesフォルダにcentos.jsonが作成される。  
そこに実行したいRecipeを記入する。  

```Ruby
{
	"run_list":[
      “recipe[myrecipe]”
    ]
}
```

## CookBook


## Attribute

http://docs.opscode.com/chef/dsl_recipe.html  

OSの種類をチェックする。

```Ruby
case node["platform"]
when "debian", "ubuntu"
  # do debian/ubuntu things
when "redhat", "centos", "fedora"
  # do redhat/centos/fedora things
end

case node["platform_family"]
when "debian"
  # do things on debian-ish platforms (debian, ubuntu, linuxmint)
when "rhel"
  # do things on RHEL platforms (redhat, centos, scientific, etc)
end

# 32bit or 64bit
node['kernel']['machine'] =~ /x86_64/ ? "x86_64" : "i386"

```


## Recipe

### Notifie

http://www.engineyard.co.jp/blog/2013/chef-recipe-lifecycle/  
http://docs.opscode.com/chef/resources.html#notifications  
notifiesというResorce内で利用出来る属性が存在する。  
これを利用して、あるリソースが完了したら、別のリソースを実行することができる  
トリガーみたいかな？？。  

```Ruby
resource "name" do
  notifies :notification, "resource_type[resource_name]", :timer
end
```

```Ruby

# こちらのリソースのアクションを実行しない設定にする
# action :enableが該当する部分である
service "apache" do
  supports :restart => true, :reload => true
  action :enable
end

# 以下のリソースを実行後にapacheのリソースを実行する設定を行っている
# notifies :reload, "service[apache]"
template "/etc/www/configures-apache.conf" do
  notifies :restart, "service[apache]"
end
```

### リソースの実行の条件(only_if, not_if)

* only_if ifと同じ  
* not_if  unlessと同じ

色々な書き方。

```Ruby
not_if "grep adam /etc/passwd", :user => 'adam'
not_if "grep adam /etc/passwd", :group => 'adam'
not_if "grep adam /etc/passwd", :environment => { 'HOME' => "/home/adam" }
not_if "grep adam passwd", :cwd => '/etc'
not_if "sleep 10000", :timeout => 10
not_if { node[:some_value] }
not_if do
    File.exists?("/etc/passwd")
end
not_if {::File.exists?("/etc/passwd")}
```

### yum もしくは apt-getでのインストール
yumやapt-getでインストールする物はすべてpackageを利用すればよい。
さらに複数パッケージをインストールするときは以下のようにfor文で記入すればよい。  

```Ruby
%w{wget gcc make}.each do |pkg|
   package pkg do
     action [:install, :upgrade]
   end
end
```

packageは他にもサポートしている。  
直接リソース名を変更することもかのだが、  
packageのproviderを変更することで同じ処理を行うことができる。

```Ruby
package "tar" do
  provider Chef::Provider::Package::Yum
  action :install
end

yum_package "tar" do
  action :install
end
```


### ソースコードからのインストール  
参考
http://stackoverflow.com/questions/8530593/chef-install-and-update-programs-from-source  
直接tar.gzファイルなどからmakeなどでインストールする場合、以下の通りにするとよい。  

```Ruby

remote_file "#{Chef::Config['file_cache_path']}/#{libevent}.tar.gz" do
  action :create_if_missing
  source   "https://github.com/downloads/libevent/libevent/#{libevent}.tar.gz"
  checksum node[:tmux][:libevent][:checksum]
  notifies :run, 'bash[install_libevent]', :immediately
end

bash 'install_libevent' do
  user 'root'
  cwd  Chef::Config['file_cache_path']
  code <<-EOH
      tar -zxf #{libevent}.tar.gz
      cd #{libevent}
      ./configure && make && make install
    EOH
  action :nothing
end

```

### ユーザーでのコマンドの実行(perlbrew, rbenvの設定)

perlbrewやrbenvはユーザー単位ので管理を行いたい
コマンド(rbenv install etc..)を

```Ruby
   execute "ruby install #{v}" do
     user node['user']
     group node['group']
     environment ( { 'RBENV_ROOT' => home } )
     path ["#{home}/bin/:$PATH"]
     command "#{home}/bin/rbenv install #{v}"
     not_if {::File.exists?("#{home}/versions/#{v}")}
   end
```

### カスタムリソース


https://wiki.opscode.com/display/chef/Lightweight+Resources+and+Providers+(LWRP)
http://docs.opscode.com/lwrp_custom_resource.html

自分独自のResourceを作成します。
作成したファイルは以下の通り

```
├── providers
│   └── test.rb
├── recipes
│   └── default.rb
├── resources
│   └── test.rb
```

* resources  
	カスタムResourceのAttributeの設定。
* providers  
	定義したResourceの実際の処理の部分。

#### resources

```Ruby
# resources/test.rb
# ここで利用出来るアクションの設定
# actions :action_name1, :action_name2, :action_name...
actions :foo, :bar

# actionが何も指定されなかったとき、実行するアクション
# default_action :action_name1
default_action :foo

# Attributeの設定
# attribute :attribute_name, :kind_of => value, :validation_parameter => value
attribute :name, :kind_of => String, :name_attribute => true
attribute :type, :kind_of => String, :default => "bar"
```

:name_attribute => trueとは、リソースの後に追加される文字列のこと
以下では”test foo”が該当する。

```Ruby
custom_resource_test "test foo" do
end
```


#### providers

new_resource.attribute_nameでresources/test.rbに
設定している

```Ruby
# providers/test.rb
# resources/test.rbで定義したアクションの処理を記述
# new_resource.nameは以下の “test foo”の部分と一致する
# custom_resource_test "test foo" do
# end

action :foo do
  p "#{new_resource.name}"
  p "#{new_resource.type}"
end

action :bar do
  p "#{new_resource.name}"
  p "#{new_resource.type}"
  bash "test" do
    user "root"
    code <<-EOH
      echo #{new_resource.type}
    EOH
  end
end
```

#### recipes

COOKBOOKNAME_FILENAMEがリソース名となる。  
FILENAMEがdefault.rbの場合、COOKBOOKNAMEがリソース名。  
今回は、custom_resourceがCOOKBOOKNAMEで  
providers/test.rbとresources/test.rbがあるため  
リソース名は`custom_resource_test`となる  

```Ruby
# recipes/default.rb
custom_resource_test "test foo" do
end

custom_resource_test "test bar" do
  action :bar
  type "bar type"
end
```

Chefのバージョンによってdefault_actionの書き方が異なるので *注意* 。
ただ現状はif defined?(default_action)がなくても動く。

```Ruby
actions :create, :destroy
default_action :create if defined?(default_action) # Chef > 10.8
 
# Default action for Chef <= 10.8
def initialize(*args)
  super
  @action = :create
end
```

### Try and Catch

[参考はこちら](http://stackoverflow.com/questions/19944517/how-to-tell-chef-to-take-an-action-if-the-desired-package-to-install-doesnt-exi)  


```Ruby
begin
   package 'some-package-name' do
     action :install
   done
rescue Chef::Exception
   # Do something here
end
```



## Test-Kitchen

複数のプラットフォームでテストができる。
あくまでレシピを実行するだけ。
設定ファイルは`.kitchen.yml`である

```Bash
$ kitchen init
	create  .kitchen.yml
   	append  Thorfile
   	create  test/integration/default
 	append  .gitignore
 	append  .gitignore
   	append  Gemfile
$ kitchen test
```

```Ruby
# .kitchen.yml
---
driver:
  name: vagrant

provisioner:
  name: chef_solo

# プラットフォームの選択
platforms:
  - name: ubuntu-12.04
  - name: centos-6.4

suites:
  - name: default
	# テストで実行したいレシピを書く
    run_list:
      - recipe[my-server::default]
      - recipe[my-server::tmux]
    attributes:
```

## ServerSpec

サーバの状態をテストするフレームワーク。
http://serverspec.org/

* OSの選択(UNIX/Windows)
* SSH or ローカル(ssh/exec)
* Vagrantのインスタンス(Yes/No)
* Vagrantfileの設定を利用する(Yes/No)

```Bash
$ serverspec-init
Select OS type:

  1) UN*X
  2) Windows

Select number: 1 # unixなので1

Select a backend type:

  1) SSH
  2) Exec (local)

Select number: 1 # sshで接続

Vagrant instance y/n: y # Vagrantのインスタンス
Auto-configure Vagrant from Vagrantfile? y/n: # Vagrantfileから設定を利用
 + spec/
 + spec/default/
 + spec/default/httpd_spec.rb
 + spec/spec_helper.rb
 + Rakefile
```

Rakefileに`rake spec`で実行するテストコードの設定を行う。

```Ruby:spec_helper.rb
require 'rake'
require 'rspec/core/rake_task'

RSpec::Core::RakeTask.new(:spec) do |t|
	#ここで実行したいテストの設定が出来る
	t.pattern = 'spec/*/*_spec.rb'
end

```

テストの書き方
http://serverspec.org/resource_types.html

```Ruby
# spec/*/*_spec.rb
# これ必須
require 'spec_helper'
# 以下テスト用のリソースを記入
describe command('echo $SHELL') do
	it { should return_stdout /\/bin\/zsh/ }
end
```

テストの実行は`rake spec`or`rspec`です。　　
以下は上のテストコードが失敗したときのログです。　　

```Bash
$ rake spec

Failures:

  1) Command "echo $SHELL" should return stdout /\/bin\/zsh/
     Failure/Error: it { should return_stdout /\/bin\/zsh/ }
       sudo echo $SHELL
       /bin/bash
       expected Command "echo $SHELL" to return stdout /\/bin\/zsh/
     # ./spec/default/zsh_spec.rb:7:in `block (2 levels) in <top (required)>'

Finished in 16.64 seconds
7 examples, 1 failure

Failed examples:

rspec ./spec/default/zsh_spec.rb:7 # Command "echo $SHELL" should return stdout /\/bin\/zsh/

```

## Berkshelf

[公式](http://berkshelf.com/)

RubyでいうGemFileみたいなものでCookbook管理ツールです。

```Ruby
site :opscode

cookbook 'mysql'
cookbook 'nginx', '~> 1.8.0'
```

新しいCookBookの作成

```Bash
$ berks cookbook new_app
$ cd new_app
$ tree .
.
├── Berksfile
├── Gemfile
├── LICENSE
├── README.md
├── Thorfile
├── Vagrantfile
├── attributes
├── chefignore
├── definitions
├── files
│   └── default
├── libraries
├── metadata.rb
├── providers
├── recipes
│   └── default.rb
├── resources
└── templates
    └── default
```

通常のcookbookに加え、VagrantFileなどが追加される。

## foodcritic

Chefのcookbookのためのlintツール  
http://acrmp.github.io/foodcritic/  
http://www.foodcritic.io/  

foodcritic [cookbook_path]

* -t, --tags TAGS  
指定されたタグに該当するルールだけをチェック  
複数のタグの場合はカンマ区切り  
(annoyances,attributes,correctness,definitions,deprecated,  
environments,etsy,files,libraries,lwrp,metadata,notifications,  
portability,processes,readme,recipe,roles,search,services,solo,  
string,style)  
* -f, --epic-fail TAGS  
ルールに反した箇所が存在した場合はエラーを返す  
* -B, —cookbook-path PATH  
チェックするCookBookのパス(省略出来る)  
* -C, --[no-]context  
ルールに反した箇所をハイライトして表示 
* -I, --include PATH  
追加のルールのファイルのパス  


カレントディレクトリ以下の構文チェックを行う

```Bash
$ foodcritic -f any .
# ルールを除外する場合は ~を利用する
$ foodcritic -f any -f ~FC014
```
