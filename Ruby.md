# Ruby

## rbenv install

[github](https://github.com/sstephenson/rbenv)

```bash
# rbenv setup
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv
git clone https://github.com/sstephenson/ruby-build.git ~/.rbenv/plugins/ruby-build
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
rbenv install 2.0.0-p353
rbenv global 2.0.0-p353
```

以下を`.zshrc`などに追記しておく。  

```
export PATH="$HOME/.rbenv/bin:$PATH"
eval "$(rbenv init -)"
```

#### インストールでのエラー

The special version name system tells rbenv to use the system Ruby (detected by searching your $PATH).
When run without a version number, rbenv global reports the currently configured global version.

/opt/vagrant_ruby/bin/がすでに$PATHに設定されているため、
削除する必要がある。(rbenv global 2.0.0-p247が反映されないため)

## windowsのインストールで失敗


```
C:\Dev-Kit> ruby dk.rb init

Initialization complete! Please review and modify the auto-generated
'config.yml' file to ensure it contains the root directories to all
of the installed Rubies you want enhanced by the DevKit.

C:\Dev-Kit> ruby dk.rb install
Invalid configuration or no Rubies listed. Please fix 'config.yml'
and rerun 'ruby dk.rb install'
```

上のエラーはconfig.ymlに

```
# Example:
#
# ---
# - C:/ruby19trunk
# - C:/ruby192dev
#
--
- C:/Ruby200-x64
```

インストールしたrubyのパスを記入すると、うまくいく。
以下成功例

```
C:\Dev-Kit>ruby dk.rb install
[INFO] Updating convenience notice gem override for 'C:/Ruby200-x64'
[INFO] Installing 'C:/Ruby200-x64/lib/ruby/site_ruby/devkit.rb'
```


## class && module

クラスは、オブジェクトとしてインスタンス化できるが
モジュールは、インスタンス化できない

継承できるのはクラス。
includeできるのはモジュールのみである


