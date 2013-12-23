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

vagrant box add [ name ] [ URL ]
作成できる仮想環境の種類を追加する
isoファイルみたいなもの。

vagrant box list
作成できるリストの一覧

色々なBox
http://www.vagrantbox.es/

## 仮想環境の基本操作

vagrant init [ name ]
作成

vagrant up
起動

vagrant halt
停止

vagrant reload
再起動

vagrant destroy
破棄

vagrant ssh
sshでログイン

vagrant ssh-config --host centos >> ~/.ssh/config
ssh centos
vagrant sshをせずに接続できる

## プラグイン

### sahara
サーバをいじっていて元に戻したいときに使う

vagrant sandbox on
sandbox モードに入る

vagrant sandbox rollback
onした状態まで巻き戻す

vagrant sandbox commit
変更内容を確定させる

### Berkshelf
ChefのCookBookとVagrantfileを同時に作ってくれる。
さらにGemfileみたいに他のCookbookを簡単にダウンロードできる

berks cookbook
cookbookの作成

berks install

##
