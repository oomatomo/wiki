Zsh
=====

zshについてのwiki

### install

yum or apt-getで簡単にインストール出来る。  

```Bash
# yum -y install zsh
# chsh -s /bin/zsh username
   or
$ chsh -s /bin/zsh
$ echo $SHELL #ログイン処理を表示
```

## 事前知識

### zshの利点

* 高速（タブ補完など）  
* カスタマイズが楽  

### 設定ファイルの読み込みについて

/etc/zshenv  
~/.zshenv  
/etc/zprofile  
~/.zprofile  
/etc/zshrc  
~/.zshrc  
/etc/zlogin  
~/.zlogin  

### 設定ファイルの種類別

* ~/.zshenv  
すべての局面で利用する設定を記述する。  
コマンド検索パスの定義    
リモートホストから直接起動する可能性のあるコマンドに関する設定  
* ~/.zshrc  
対話的に起動する場合にのみ必要な設定。  
* ~/.zlogin   
ログイン時にただの一度だけ行えばよい設定。  


ショートカット
---

### 移動

* Ctrl+a # 先頭へ  
* Ctrl+e # 行末へ  
* Ctrl+p #カーソル上  
* Ctrl+n #カーソル下  
* Ctrl+f #カーソル右  
* Ctrl+b #カーソル左  

### 文字列操作

* Ctrl+u # その行全部切り取り  
* Ctrl+y # ペースト  
* Ctrl+/ # 一つ前に元に戻る  
* Ctrl+k # カーソルから行末までの文字を切り取り  
* Ctrl+h # 左の文字を削除  
* Ctrl+d # 右の文字を削除  
* Ctrl+w # 左の単語を切り取り  
* Meta+d # 右の単語を切り取り  


.zshrc
---

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
setopt 設定したい値
unsetopt 無効にしたい値
```

### 色々なシェルオプション

```Bash
setopt auto_menu            # タブで補完候補を表示する
setopt auto_cd              # ディレクトリ名のみ入力時、cdを適応させる
setopt auto_list            # 補完候補が複数ある時に、一覧表示
setopt auto_pushd           # cd実行時、ディレクトリスタックにpushされる
setopt auto_param_keys      # カッコの対応などを自動的に補完
setopt auto_param_slash     # ディレクトリ名の補完で末尾に/を自動的に付加
setopt correct              # コマンドのスペルを訂正する
setopt equals               # =commandを`which command`と同じ処理
setopt globdots             # ドットの指定なしでドットファイルも候補に入る
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

### バインドキー

zshの操作方法には2種類ある。  
viモードとemacsモードです。デフォルトはemacsモードです。  

```Bash
# モード選択
bindkey -e # emacs
bindkey -v # vi
```

キーの割り当て確認は以下のコマンドを実行すると良い。  

```Bash
bindkey -M emacs
bindkey -M vicmd
bindkey -M viins
# 別途で補完メニュー選択時のキーバインドも存在する
bindkey -M menuselect 
```

bindkeyはの設定の方法はいくつかある。

```Bash
bindkey -M 'モード(vi?emacs?)' '設定したいキー'　'実行する関数'
# Ctrl+aでend-of-line(行末に移動する)が実行される。
bindkey -M -e '^a' end-of-line
```

### vicmd と viins　の違い

vicmdはviのノーマルモード、viinsはインサートモード

```Bash
# emacs
bindkey -M emacs '設定するキー' '実行する関数'
# vicmd時のバインドキーの設定
bindkey -M vicmd '設定するキー' '実行する関数'
# viins時のバインドキーの設定
bindkey -M viins '設定するキー' '実行する関数'
```

viモードのステータスを分かりやすくする

[zshのViモードをインターフェイスとして利用してみる](http://qiita.com/PSP_T/items/8cc534c2c30543965950)

```Bash
function zle-line-init zle-keymap-select {
　　　VIM_NORMAL="%K{208}%F{black}⮀%k%f%K{208}%F{white} % NORMAL %k%f%K{black}%F{208}⮀%k%f"
    VIM_INSERT="%K{075}%F{black}⮀%k%f%K{075}%F{white} % INSERT %k%f%K{black}%F{075}⮀%k%f"
    RPS1="${${KEYMAP/vicmd/$VIM_NORMAL}/(main|viins)/$VIM_INSERT}"
    RPS2=$RPS1
    zle reset-prompt
}
zle -N zle-line-init
zle -N zle-keymap-select
```

### 補完メニューでのキーバインド

```
zmodload zsh/complist
bindkey -M menuselect 'h' vi-backward-char
bindkey -M menuselect 'j' vi-down-line-or-history
bindkey -M menuselect 'k' vi-up-line-or-history
bindkey -M menuselect 'l' vi-forward-char
```

## zle

自分でカスタマイズした関数や既存の関数をzshで呼び出せるようにします。
ちなみにzleで機能しているものの単位をウィジェットといいます。

```Bash

zle -N WIDGET [FUNCTION]
# ウィジェットと関数名を同一にしたくないなら第2引数に関数名を記入

```

### 履歴関連

```Bash
HISTFILE=~/.zsh_history     # ヒストリファイルを指定
HISTSIZE=10000              # ヒストリに保存するコマンド数
SAVEHIST=10000              # ヒストリファイルに保存するコマンド数

# 履歴をたどる
autoload -Uz history-search-end
bindkey -M vicmd 'k' history-beginning-search-backward
bindkey -M vicmd 'j' history-beginning-search-forward

# ワイルドカードのインクリメントサーチが可能
bindkey -M vicmd 'll' history-incremental-pattern-search-backward
bindkey -M vicmd 'LL' history-incremental-pattern-search-forward

# 途中入力のコマンドを元に履歴をたどる
bindkey -M vicmd '^P' up-line-or-history
bindkey -M vicmd '^N' down-line-or-history

# 途中入力のコマンドの元に履歴をたどる
autoload -Uz up-line-or-beginning-search down-line-or-beginning-search
zle -N up-line-or-beginning-search
zle -N down-line-or-beginning-search
bindkey -M vicmd '^P' up-line-or-beginning-search
bindkey -M vicmd '^N' down-line-or-beginning-search
```

履歴に関してのシェルオプション

```Bash
# 履歴のシェルオプション
setopt bang_hist            # !を使った履歴展開を行う(d)
setopt extended_history     # 履歴に実行時間も保存する
setopt hist_ignore_dups     # 直前と同じコマンドは履歴に追加しない
setopt hist_reduce_blanks   # 余分なスペースを削除して履歴に保存する
setopt hist_no_store        # historyコマンドは履歴に登録しない
setopt hist_expand          # 補完時に履歴を自動的に展開
setopt hist_ignore_dups     # 入力したコマンドが直前のものと同一なら履歴に登録しない
setopt hist_save_no_dups    # 入力したコマンドが直前のものと同一なら古いコマンドのほうを削除する
setopt hist_find_no_dups    # ラインエディタでヒストリ検索し、ヒットした場合でも重複したものとみなさない
setopt hist_ignore_all_dups # 入力したコマンドを履歴に登録する時、同一がすでに存在した場合登録しない
setopt hist_no_functions    # 関数定義のためのコマンドは履歴から削除する
setopt hist_no_store        # 履歴参照のコマンドは履歴に登録しない
setopt hist_reduce_blanks   # コマンド中の余分な空白を削除する
setopt inc_append_history   # 履歴をインクリメンタルに追加
setopt share_history        # 他のシェルのヒストリをリアルタイムで共有する

```


### 補完機能の設定

```Bash
# 必須
autoload -U compinit; compinit
```

zstyleでの補完機能の以下の構文となっている。  

```Bash
# 3種類の設定する部分がある
zstyle Pattern Style Value
# Patternはさらに詳細に分割できる 　　
zstyle ':completion:function:completer:command:argument:tag' style value
# 実際の設定
zstyle ':completion:*:rm:*' menu true
zstyle ':completion:*:使うコマンド:*' スタイル 値
```

### Pattern

* completion 補完システムの設定を行うための固定文。  
* function ウィジェットからの関数名  
* completer  補完ならcomplete　展開ならexpand
* command  補完のコマンド
* argument  引数
* tag  タグ

ただし実際の記入する際は短縮することが出来る。  

```Bash

# 一覧表示でグループ化を行う
zstyle ':completion:*' group-name ''
# グループ化での説明を追加出来る %dに説明が入る
zstyle ':completion:*:descriptions' format 'Completing %d'

zstyle ':completion:*' list-separator '-->' 				# オプションの補完時のデザイン
zstyle ':completion:*:manuals' separate-sections true 		# 設定したデザインの表示を許可する

zstyle ':completion:*:setopt:*' menu true select 			# 補完のメニューが表示されるかつ選択を行うことができる
zstyle ':completion:*:options' description 'yes'			# オプションの説明をONにする
# 以下補完時のメッセージに関してのフォーマット
zstyle ':completion:*:messages' format '%F{YELLOW}%d'$DEFAULT
zstyle ':completion:*:warnings' format '%F{RED}No matches for:''%F{YELLOW} %d'$DEFAULT
zstyle ':completion:*:descriptions' format '%F{YELLOW}Completing %B%d%b%f'$DEFAULT

```

### どんな設定があるか確認する方法


### カラー

```Bash
# 色の有効化
autoload -U colors; colors

# 色の設定
zstyle ':completion:*:default*' list-colors di=4 ex=33
# もしくは
export LS_COLORS='di=4 ex=33'
zstyle ':completion:*:default*' list- ${(s.:.)LS_COLORS}
```

* no        標準色    
* fi        通常ファイル  
* di        ディレクトリ  
* ex        実行ファイル  
* ln        シンボリックリンクファイル  
* pi        パイプファイル  
* so        ソケットファイル  
* bd        ブロックデバイス
* cd        キャラクタデバイス  
* tc        ファイルの種別を示す記号  
* sp        候補単語間の空白  
* =パターン パターンにマッチする候補  


色の確認

```Bash
for c in {000..255}; do echo -n "\e[38;5;${c}m $c" ; [ $(($c%16)) -eq 15 ] && echo;done;echo
```

### 参考

https://wiki.archlinux.org/index.php/Zsh_(%E6%97%A5%E6%9C%AC%E8%AA%9E)

### oh-my-zsh


### install

https://github.com/robbyrussell/oh-my-zsh  

```Bash
$ curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh
$ source ~/.zshrc
```

$HOME/.oh-my-zshに生成される。
以下のフォルダが作成される。

```
.oh-my-zsh
├── MIT-LICENSE.txt
├── README.textile
├── cache
├── custom
├── lib
├── log
├── oh-my-zsh.sh
├── plugins
├── templates
├── themes
└── tools

```

### theme

テーマの一覧表示

```Bash
$ ZSH=~/.oh-my-zsh ./theme_chooser.sh -s
```

テーマの変更(~/.zshrcを編集)
ZSH_THEMEの値を変更するといい。
テーマは`.oh-my-zsh/themes/`の以下にある。

`テーマ名.zsh-theme`となっている・

```Bash
ZSH_THEME="ooma"
```

### plugins

oh-my-zshにはプラグインが存在する。使い方は簡単である。  
~/.zshrcを編集する。

```Bash
plugins=( プラグイン名 )
```

これでおｋ

### カスタマイズ

.oh-my-zsh/custom以下のとなる  

本来の.zshrcの設定を行いたい場合は  
`.oh-my-zsh/custom/custom.zsh`に記入する  

theme  
`.oh-my-zsh/custom/themes/`にファイルを追加する  
`プラグイン名.zsh-theme`となる  
`ooma.zsh-theme`とするとテーマ名はoomaとなる  

plugin  
`.oh-my-zsh/custom/plugins/`へファイルを追加する。  

###  zsh-syntax-highlighting

コマンドをカラフルにしてくれます。

https://github.com/zsh-users/zsh-syntax-highlighting

デフォルトのプラグインではないため、`.oh-my-zsh/custom/plugins`にcloneする。

```Bash
$ cd ~/.oh-my-zsh/custom/plugins
$ git clone git://github.com/zsh-users/zsh-syntax-highlighting.git
```

~/.zshrcを編集する。

```Bash
plugins=( zsh-syntax-highlighting )
```

### zsh-autosuggestions

自動でコマンドラインの補完を行ってくれる

https://github.com/tarruda/zsh-autosuggestions

```Bash
$ git clone git://github.com/tarruda/zsh-autosuggestions ~/.oh-my-zsh/custom/zsh-autosuggestions
```

.oh-my-zsh/custom/custom.zshの変更を行う

```
source ~/.oh-my-zsh/custom/zsh-autosuggestions/autosuggestions.zsh

zle-line-init() {
    zle autosuggest-start
}
zle -N zle-line-init

bindkey '^T' autosuggest-toggle
```
