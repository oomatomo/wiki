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

## .zshrc

設定を記入する

### setopt

```Bash


```

### 補完機能の設定

```Bash
# 必須
autoload -U compinit; compinit
```

zstyleでの補完機能の以下の構文となっている。  
`zstyle ':completion:function:completer:command:argument:tag’`　　

* completion  
補完システムの設定を行うための固定文。  
* function   
ウィジェットからの関数名  
* completer  
* command  
* argument  
* tag  



```Bash
# 一覧表示でグループ化を行う
zstyle ':completion:*' group-name ''
# グループ化での説明を追加出来る %dに説明が入る
zstyle ':completion:*:descriptions' format 'Completing %d' 

```


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

