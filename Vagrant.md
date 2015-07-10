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

### windowsでネット接続が遅い場合

以下の設定で解消できる  

```
  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
  end
```

参考URL  
[vagrant-slow-internet-connection-in-guest](http://serverfault.com/questions/495914/vagrant-slow-internet-connection-in-guest)  
[vagrant-virtualbox-dns-10-0-2-3-not-working](http://serverfault.com/questions/453185/vagrant-virtualbox-dns-10-0-2-3-not-working)  


### Disk追加( VirtualBox )


```
file_to_disk = './box-disk3.vmdk'
unless File.exist?(file_to_disk)
  vb.customize [ "createhd", "--filename", file_to_disk, "--size", 80*1024 ]
end
vb.customize ['storageattach', :id, '--storagectl', 'SATA', '--port', 4, '--device', 0, '--type', 'hdd', '--medium', file_to_disk]

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

## Macで

```
Failed to mount folders in Linux guest. This is usually because
the "vboxsf" file system is not available. Please verify that
the guest additions are properly installed in the guest and
can work properly. The command attempted was:

mount -t vboxsf -o uid=`id -u vagrant`,gid=`getent group vagrant | cut -d: -f3` vagrant /vagrant
mount -t vboxsf -o uid=`id -u vagrant`,gid=`id -g vagrant` vagrant /vagrant

The error output from the last command was:
/sbin/mount.vboxsf: mounting failed with the error: No such device
```

```
vagrant plugin install vagrant-vbguest
```

vagrant-vbguestというプラグインをインストールすればおk
