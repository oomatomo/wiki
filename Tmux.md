title: Tmux
output: tmux.html

--
# tmux
--
### tmuxとは

* 一つのターミナルでタブ化できる  
* ターミナルの画面を分割することができる  
* デタッチ,アタッチができる  

--
### デタッチ,アタッチ

* デタッチ：  
作業中の状態(vimを開いたままなど)を*保持したまま*セッションを終了することが出来る  
* アタッチ：  
保持された状態から*再開*出来る  

--
ターミナルからサーバへセッションが切れたとしても  
サーバ側でセッションの途中の内容を保存してくれるため  
セッションが切れた直前の状態から作業ができる  

--
### Install

```bash
$ yum install wget gcc make ncurses ncurses-devel
$ apt-get install build-essential checkinstall wget libncurses5-dev
```

--
#### libevent

```bash
## libevent install
$ wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz
$ tar xvf libevent-2.0.21-stable.tar.gz
$ cd libevent-2.0.21-stable
$ ./configure
$ make
$ sudo make install
```

--
#### tmux

```bash
## tmux install
$ wget http://downloads.sourceforge.net/tmux/tmux-1.8.tar.gz
$ tar xvzf tmux-1.8.tar.gz
$ cd tmux-1.8
$ ./configure
$ make
$ sudo make install
```

#### centos6.5でエラー
以下の方法で./configureを行ったらインストールできた

```bash
# libevent2の指定
./configure --prefix=/usr/local
 
# tmuxの指定
LDFLAGS="-L/usr/local/lib -Wl,-rpath=/usr/local/lib" ./configure --prefix=/usr/local
```

--
###  InstallでのError

```bash
control.c:63: warning: implicit declaration of function ‘evbuffer_readln’
control.c:63: error: ‘EVBUFFER_EOL_LF’ undeclared (first use in this function)
control.c:63: error: (Each undeclared identifier is reported only once
control.c:63: error: for each function it appears in.)
```

libeventのバージョンが古い状態  

--
### 操作方法

```bash
## 起動
$ tmux
## アタッチ(すでに起動中のtmuxのセッションがあれば起動出来る)
$ tmux a 
$ tmux attach
## バージョンチェック
$ tmux -V
```

設定ファイルは`~/.tmux.conf`
--
### プレフィックスキー
tmuxの操作はプレフィックスキーとそれに続くコマンドキーで操作します
tmuxのデフォルトのプレフィックスキーは `Ctrl-b` です  
そのため操作を行う際は `Ctrl-b` + `コマンドキー` となる  

--
### キー操作
--
### ウィンドウ関連
```bash
# ウィンドウの作成
C-b c
# 前のウィンドウへ
C-b p
# 次のウィンドウへ
C-b n
# 直前のウィンドウに移動
C-b l
# 数字で指定したウインドウへ移動
C-b [数字]
# ウィンドウの強制終了
C-b &
# デタッチ
C-b d
```
ウインドウの一覧は `list-window`
--
### パネル関連

```bash
# 水平方向に分割
C-b "
# 垂直方向
C-b %
# 次のペインに移動
C-b o
# 縦分割を横分割へ
C-b Space
```
--
###　コピーモード
コピーモードには`C-b [`で入ることが出来る  
Spaceキーでコピーの始点を設定し、Enterでコピーの終点を決める  
貼付けは`C-b ]`で出来る  

--
### コマンド

`C-b :コマンド`　でtmuxのコマンドを実行することが出来ます 

C-b :set-window-option synchronize-panes on
現在開いているウウィンドウ内のパネルが全て同期します

--
### 参考

http://d.hatena.ne.jp/eco31/20101126/1290725841

--
### tmuxの設定
設定ファイルは`~/.tmux.conf`  
プレフィックスキーの変更  
[設定のwiki](https://bytebucket.org/ns9tks/tmux-ja/wiki/tmux-ja.html)

```bash
set-option -g prefix C-q
unbind-key C-b
bind-key C-q send-prefix
```
--
#### 設定方法
* set-option ( setでも可 )  
セッションオプションを設定します  
引数  
-g グローバルなセッションオプションに設定  
-u 設定を解除する  
* set-window-option ( setwでも可 )   
ウィンドウオプションを設定します  
引数はset-optionと同じ  

--
#### ステータスライン

色の設定は`#[fg=white,bg=red]`で出来る

```bash
set -g status-left ""
set -g status-right ""
set -g window-status-format ""
```
--
#### cmatrix

```bash
# Screensaver
set -g lock-after-time 600
set -g lock-command "cmatrix -s -b"
```
--
