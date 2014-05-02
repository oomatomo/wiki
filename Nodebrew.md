
# Nodebrew

## install

```
curl -L git.io/nodebrew | perl - setup
```

```
export PATH=$HOME/.nodebrew/current/bin:$PATH
```

```
nodebrew install-binary stable
nodebrew use stable
```

グローバルでインストールする際は以下の通りにしてください
```
npm install -g coffee-script
```

## uninstall system node

for Ubuntu 12.04
$ sudo apt-get remove nodejs


## grunt

http://stackoverflow.com/questions/10667381/node-package-grunt-installed-but-not-available
```
npm install -g grunt
# grunt-cliもインストールしないとgruntコマンド実行できない
npm install -g grunt-cli
```
