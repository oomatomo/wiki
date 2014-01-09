# tmux

## tmuxとは

* 一つのターミナルでタブ化できる  
* ターミナルの画面を分割することができる  
* デタッチ,アタッチができる  

### デタッチ,アタッチ

デタッチ：作業中の状態(vimを開いたままなど)を保持したままセッションを終了することが出来る  
アタッチ：保持された状態から再開出来る  

ターミナルからサーバへセッションが切れたとしても  
サーバ側でtmuxのセッションの途中の内容が保存されるため  
セッションが切れた直前の状態から作業ができる  

## Install

必要な物  

```Bash
# yum install wget gcc make ncurses ncurses-devel
# apt-get install build-essential checkinstall wget libncurses5-dev
```

```Bash
## libevent install
$ wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz
$ tar xvf libevent-2.0.21-stable.tar.gz
$ cd libevent-2.0.21-stable
$ ./configure
$ make
$ sudo make install

## tmux install
$ wget http://downloads.sourceforge.net/tmux/tmux-1.8.tar.gz
$ tar xvzf tmux-1.8.tar.gz
$ cd tmux-1.8
$ ./configure
$ make
$ sudo make install
```

###  InstallでのError

```Bash
control.c:63: warning: implicit declaration of function ‘evbuffer_readln’
control.c:63: error: ‘EVBUFFER_EOL_LF’ undeclared (first use in this function)
control.c:63: error: (Each undeclared identifier is reported only once
control.c:63: error: for each function it appears in.)
```

libeventのバージョンが古い状態  

## 操作方法

```Bash
## 起動
$ tmux
## アタッチ(すでに起動中のtmuxのセッションがあれば起動出来る)
$ tmux a 
$ tmux attach
## バージョンチェック
$ tmux -V
```

## キー操作

tmuxは、

```Bash


```


