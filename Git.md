# Git

## Install

yumでインストールする

```bash
$ sudo yum install git
```

ソースコードからインストールする

[コードはこちら](https://code.google.com/p/git-core/downloads/list)

今回は git-1.8.5.3.tar.gzをインストールしました  

すでにyumでインストールを行っている場合はyumでgit-coreをremoveする  

```
$ sudo yum remove git-core
```

インストールは以下の通りです  

```
$ sudo yum install openssl097a.x86_64 openssl-perl.x86_64 curl-devel
$ wget https://git-core.googlecode.com/files/git-1.8.5.3.tar.gz
$ tar zxf git-1.8.5.3.tar.gz
$ cd git-1.8.5.3 && ./configure && make && sudo make install
```

`openssl1097a.x86_64`と`openssl-perl.x86_64`は以下のエラーが出たためインストールを行った  


```
$ git clone https://github.com/oomatomo/perl-test.git
Cloning into 'perl-test'
fatal: Unable to find remote helper for 'https'
```

## Config

```
# Globalな設定
$ git config --global user.name "oomaotmo"
$ git config --global user.email oomatomo@gmail.com

# remoteのURL変更
$ git remote set-url origin URL
```

## Log

```Bash
$ git log --oneline --decorate --graph --branches --tags --remotes
```

* --oneline: 1commit 1行のみログ表示
* --decorate: branch名、tag名などの別名を表示
* --graph: revision graphを表示
* --branches: 他のbranchのlogも表示
* --tags: tagを表示
* --remotes: remote branchなどを表示

```Bash
$ git log --graph --all --color --pretty='%x09%h %cn%x09%s %Cred%d%Creset'
```
* --graph: ツリー表示が付く
* --all: 全てのログ。カレントのブランチだけに限らず。
* --color: 色
* --pretty: ログのフォーマット。%dが branch。

## Tag


```Bash
# TAG_NAMEへcheckout
$ git checkout TAG_NAME
```

## rebase

### 中止　もしくは　再開

```
$ git rebase --abort

$ git rebase --continue
```

### コミットを一つに纏める

```
$ git rebase -i HEAD~3 #この場合は3つのコミットを纏めることになる
```

```
pick 1ejdwoeijf <message>
pick lawiejfoie <message>
pick awjfojerof <message>
```

pickは残すコミットになるちなみに一番上が親のコミットである
今回は3つのコミットを一つに纏めるため下二つのコミットをsquashで
前のコミットとしてコミットする

```
pick 1ejdwoeijf <message>
squash lawiejfoie <message>
squash awjfojerof <message>
``` 

この後は通常のコミットメッセージ画面となる

## Error

### error: RPC failed; result=22, HTTP code = 405

`git clone`を行ったとき、上記のエラーが出る  
原因はcloneを行うURLにHTTPを利用しているから。HTTPSを利用するとうまくいく。  

```Bash
# Error
$ git clone http://*
# Success
$ git clone https://*
```

