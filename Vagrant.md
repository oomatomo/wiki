Vagrant

## Vagrantとは
rubyで書かれたVirtual Boxのフロントエンドツールみたいなもの。
コマンドでVMの作成、起動、破棄ができる。

## 事前準備
Virtual Box
https://www.virtualbox.org/wiki/Downloads
Vagrant
http://downloads.vagrantup.com/

## 仮想環境の追加

```
#作成できる仮想環境の種類を追加する
#isoファイルみたいなもの。
$ vagrant box add [ name ] [ URL ]
#作成できるリストの一覧
$ vagrant box list
```

色々なBox
http://www.vagrantbox.es/

## 仮想環境の基本操作

```
#作成
$ vagrant init [ name ]
#起動
$ vagrant up
#停止
$ vagrant halt
#再起動
$ vagrant reload
#破棄
$ vagrant destroy
#sshでログイン
$ vagrant ssh
#vagrant sshをせずに接続できる
$ vagrant ssh-config --host centos >> ~/.ssh/config
$ ssh centos
```

## Vagrantfile

仮想環境の設定ファイルである。

```
# 以下が設定ファイルとなる
Vagrant.configure("2") do |config|
	# VMのホスト名
	config.vm.hostname = "my-server"

	# vagrantのboxの名前
	config.vm.box = "CentOS-6.4-x86_64-v20130731"
	# このBoxのURL
	# ローカルにconfig.vm.boxで設定したBox名が存在しない場合、以下のURLからBoxを取得する
	config.vm.box_url = "http://developer.nrel.gov/downloads/vagrant-boxes/CentOS-6.4-x86_64-v20130731.box"

	# 
	# 同期させるフォルダの選択 以下では 
	# “../data”がホスト側 "/vagrant_data" VN側となる
	config.vm.synced_folder "../data", "/vagrant_data"
	# providerの設定（今回はVirtualbox）
	config.vm.provider :virtualbox do |vb|
		# GUIの画面を表示
		vb.gui = true	
		# virtualbox側にでるサーバの名前
		vb.name = "my-server"
		# メモリ変更
		vb.customize ["modifyvm", :id, "--memory", "2048"]
	end

end
```

## プラグイン  

```
$ vagrant plugin install plugin_name
```
  
### sahara
サーバをいじっていて元に戻したいときに使う  

```
$ vagrant plugin install sahara
#sandbox モードに入る
$ vagrant sandbox on
#sandboxモードをOffにする
$ vagrant sandbox off
#sandboxモードのステータス
$ vagrant sandbox status
#onした状態まで巻き戻す
$ vagrant sandbox rollback
#変更内容を確定させる
$ vagrant sandbox commit
```

### vagrant-omnibus  
ChefがBoxにインストールされているか確認するプラグイン

```
$ vagrant plugin install vagrant-omnibus
```

### Berkshelf  
ChefのCookBookとVagrantfileを同時に作ってくれる。　　
さらにGemfileみたいに他のCookbookを簡単にダウンロードできる。  　　
詳しくはChef.mdのほうで説明する。  

```
$ vagrant plugin install vagrant-berkshelf
```

```Vagrantfile
Vagrant.configure("2") do |config|
	# berkshelfの使用を許可する
	config.berkshelf.enabled = true

	# Berksfileの場所を指定出来る
	config.berkshelf.berksfile_path = "./Berksfile"
	# Berksfileのここに記入　別ファイルとして作成しても可能
	# こちらを利用する場合config.berkshelf.berksfile_pathはいらない
  	File.open('Berksfile', 'w').write <<-EOS
    	cookbook ‘yum’
  	EOS

	# ここで実行するレシピの選択を行う
  	config.vm.provision :chef_solo do |chef|
    	chef.add_recipe “yum”
  	end
end

```

### vagrant-serverspec

serverspec用のプラグインである。

```
$ vagrant plugin install vagrant-serverspec
```

以下の設定を行うと`vagrant up`のときに
`spec/default/*_spec.rb`に該当するファイルのテストを行う

```
Vagrant.configure("2") do |config|
	・・・・・
	config.vm.provision :serverspec do |spec|
		spec.pattern = "spec/default/*_spec.rb"
  	end
end
```


