## Guardとは
ファイルの変更イベントを利用できるツールです。
コードが変更されたら、そのコードに該当するテストを実行するなどなど。  
色々なことが出来ます。  

対象環境    
 CentOS 6.5  
 rbenv  0.4.0-89-g14bc162    
 Ruby   2.0.0-p357    
  
###　インストール

```bash
$ gem i guard 
```

以下はインストール時のエラーについて  

```bash
which: no notify-send in (.....)
```

これに関してはファイルの変更イベントを送信できないエラーかと思われます。  

```bash
# For CentOS
$ sudo yum install libnotify
# For Ubuntu 
$ sudo apt-get install libnotify-bin
```

でうまくいきました。

次は、machine-idの生成です。
これに関しては、詳しくは知りません。

```bash
 D-Bus library appears to be incorrectly set up;
 failed to read machine uuid: Failed to open "/var/lib/dbus/machine-id":
```

```bash
sudo yum install dbus
sudo dbus-uuidgen > /var/lib/dbus/machine-id
```

これでOKです。  

## 基本的な流れ

```bash
$ guard init
$ guard
```

基本的な流れは以上です。  

### guard init

これでGuardfileが作成されます。  
guardの設定は全てGuardfileに設定できます。  

**Guradのプラグインがインストールされていた場合**  
**Guardfileに全てのプラグインのDefaultの設定が反映されます**

```bash
$ guard init
10:33:21 - INFO - Writing new Guardfile to path/Guardfile
```

```
# A sample Guardfile
# More info at https://github.com/guard/guard#readme 
```

### guard 

guardコマンドでファイル監視を行います。  

```bash
$ guard
10:43:49 - ERROR - No plugins found in Guardfile, please add at least one.
10:43:49 - INFO - Guard is using NotifySend to send notifications.
10:43:49 - INFO - Guard is using Tmux to send notifications.
10:43:49 - INFO - Guard is using TerminalTitle to send notifications.
10:43:49 - INFO - Guard is now watching at 'path' 
[1] guard(main)> 
```

現状はエラーが出ています。  
理由としては、Guardfileに何も書かれていないからです。  

当たり前ですね。  

## Guard Plugins

guardにはたくさんのプラグインがあります。  
https://github.com/guard/guard/wiki/List-of-available-Guards  

#### Plugins list

guard list でインストールされているプラグインが確認できます。

```
$ guard list
  +--------+-----------+
  | Plugin | Guardfile |
  +--------+-----------+
  | Rspec  | ✔        |
  +--------+-----------+
```


### Guardfile

GuardにはPluginがあり、GuardfileにPluginを使用する設定を記入します。

**すでにgemでプラグインがインストールされている場合は**  
**インストールされたプラグインの設定ファイルが`guard init`時に追記されます。**  
プラグインは`guard init プラグイン名`で指定できます

```
# rspecの設定がGuardfileに反映される
$ guard init rspec
```

```
guard :プラグイン名 do
  # 設定
  watch(/監視したいファイル/) { 実行したいコマンド}
　# m[0]には監視されているファイル名が入る
　watch(/監視したいファイル/) {|m| m[0]}
end
```

### guard-shell

https://github.com/guard/guard-shell  

これはとてもシンプルで好きです。  

```
guard :shell do
  watch(/(.*).t$/) {|m| `carton exec -- prove #{m[0]}` }
end
```

rubyではなくPerlになりますが。。  
行っていることは  
　.tファイルが変更されたらそのファイルのテストを実行する  
　( `test.t`が変更されたら `carton exec -- prove test.t`が実行されます )


### guard-rspec

rspec用のguardプラグイン


