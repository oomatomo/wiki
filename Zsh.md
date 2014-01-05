# Zsh

## install

yum or apt-getで簡単にインストール出来る。  

```Bash
# yum -y install zsh
# chsh -s /bin/zsh username
   or
$ chsh -s /bin/zsh
$ echo $SHELL #ログイン処理を表示
```

## ショートカット

### 移動

Ctrl+a # 先頭へ
Ctrl+e # 行末へ
Ctrl+p #カーソル上
Ctrl+n #カーソル下
Ctrl+f #カーソル右
Ctrl+b #カーソル左

### 文字列操作

Ctrl+u # その行全部切り取り
Ctrl+y # ペースト
Ctrl+/ # 一つ前に元に戻る
Ctrl+k # カーソルから行末までの文字を切り取り
Ctrl+h # 左の文字を削除
Ctrl+d # 右の文字を削除
Ctrl+w # 左の単語を切り取り
Meta+d # 右の単語を切り取り


## .zshrc

設定を記入する

### プロンプト設定

* PROMPT 通常のプロンプト
* RPROMPT 右側のプロンプト
* PROMPT2 2行目以降のプロンプト
* SPROMPT 存在しないコマンド実行時の「もしかしてプロンプト」

### シェルオプション

シェルオプションで簡単に設定可能。  

* setopt　オプションの有効
* unsetopt　オプションの無効

```Bash
setopt auto_menu            # タブで補完候補を表示する
setopt auto_cd              # ディレクトリ名のみ入力時、cdを適応させる
setopt auto_correct         # 補完時にスペルチェックをする
setopt auto_list            # 補完候補が複数ある時に、一覧表示
setopt auto_pushd           # cd実行時、ディレクトリスタックにpushされる
setopt auto_param_keys      # カッコの対応などを自動的に補完
setopt auto_param_slash     # ディレクトリ名の補完で末尾に/を自動的に付加
setopt correct              # コマンドのスペルを訂正する
setopt equals               # =commandを`which command`と同じ処理
setopt globdots             # ドットの指定なしでドットファイルも候補に入る
setopt hist_expand          # 補完時にヒストリを自動的に展開する
setopt interactive_comments # コマンドラインでの#以降をコメントと見なす
setopt list_types           # 候補にファイルの種別を表示(ls -F)
setopt list_packed          # 補完結果をできるだけ詰める
setopt no_beep              # ビープ音を鳴らさない
setopt nolistbeep           # 補完候補表示時にビープ音を鳴らさない
setopt no_tify              # バックグランドジョブが終了時知らせてくれる
setopt magic_equal_subst    # 引数での=以降も補完(--prefix=/usrなど)
setopt mark_dirs            # ファイル名の展開でディレクトリにマッチした場合末尾に / を付加する
setopt prompt_subst         # プロンプト定義内で変数置換やコマンド置換を扱う
setopt print_exit_value     # 戻り値が 0 以外の場合終了コードを表示
setopt pushd_ignore_dups    # ディレクトリスタックに重複する物は古い方を削除
```

### 履歴関連

```Bash
HISTFILE=~/.zsh_history     # ヒストリファイルを指定
HISTSIZE=10000              # ヒストリに保存するコマンド数
SAVEHIST=10000              # ヒストリファイルに保存するコマンド数
setopt hist_ignore_dups     # 履歴の重複する行を無視
setopt bang_hist            # !を使ったヒストリ展開を行う(d)
setopt extended_history     # ヒストリに実行時間も保存する
setopt hist_ignore_dups     # 直前と同じコマンドはヒストリに追加しない
setopt share_history        # 他のシェルのヒストリをリアルタイムで共有する
setopt hist_reduce_blanks   # 余分なスペースを削除してヒストリに保存する
```

### 補完機能の設定

```Bash
# 必須
autoload -U compinit; compinit
```

zstyleでの補完機能の以下の構文となっている。  
`zstyle ':completion:function:completer:command:argument:tag’`　　

* completion 補完システムの設定を行うための固定文。  
* function ウィジェットからの関数名  
* completer  補完ならcomplete　展開ならexpand
* command  補完のコマンド
* argument  引数
* tag  タグ


```Bash
# 一覧表示でグループ化を行う
zstyle ':completion:*' group-name ''
# グループ化での説明を追加出来る %dに説明が入る
zstyle ':completion:*:descriptions' format 'Completing %d' 
```

### バインドキー

zshの操作方法には２種類ある。
viモードとemacsモードです。

```Bash

```

## 参考

[archlinux](https://wiki.archlinux.org/index.php/Zsh_(%E6%97%A5%E6%9C%AC%E8%AA%9E))

## oh-my-zsh

### install

https://github.com/robbyrussell/oh-my-zsh  

```Bash
$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
```


### theme

テーマの一覧表示

```Bash
$ ./theme_chooser.sh -s
```

### color

カラー一覧を見ることができるシェルスクリプト  

```Bash
for i in $(seq 0 8 255); do
  for j in $(seq $i $(expr $i + 7)); do
    for k in $(seq 1 $(expr 7 - ${#j})); do
            printf " "
    done
    printf "\x1b[38;5;${j}mcolour${j}"
    [[ $(expr $j % 8) != 7 ]] && printf "    "
  done
  printf "\n"
done
```