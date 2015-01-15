# Docker

## 公式が一番

[公式](http://docs.docker.io/en/latest/installation/windows/)

## 準備

### DockerがインストールされているVagrantfileを実行

```
git clone https://github.com/dotcloud/docker.git
cd docker
vagrant up
```
### Centos Docker install

```
# CentOS 7
yum install docker 
# CentOS 6
yum install docker-io

service docker start
chkconfig docker on
```
### Windows Docker install

boot2dockerをインストールします
https://docs.docker.com/installation/windows/

ssh-keygen　がないエラーの場合 msysgitをインストールする
http://msysgit.github.io/
PATHを追加する C:\Program Files (x86)\Git\bin

```
boot2docker init
boot2docker up
```

#### puttyでsshする

boot2dockerで利用しているIPをチェックする

```
> boot2docker ip
The VM's Host only interface IP address is: 192.168.59.103
```

puttyの設定

接続先を docker@192.168.59.103
鍵は $HOME/.ssh/id_boot2docker　を puttyのkeygenでppkに変換したものを使う

#### 設定のカスタマイズ

boot2docker config > $HOME/.boot2docker/profile

設定ファイルが出力される

```
> boot2docker -h
config error: Near line 1, key '��V
```

上のエラーの時は、文字コードの問題
デフォルト文字コードがUnicodeなのでUTF-8に変換する

boot2docker -h でヘルプ画面が表示されたら、問題なし

## Puttyからの接続

```
vagrant ssh-config

```

putty用に鍵を生成する。
PuTTYgen -> 既存の秘密鍵の読込。


## Dockerのバージョンを確認  
```
$ docker version
```

## Docker イメージを検索する  
```
$ docker search tutorial
NAME                           DESCRIPTION                                     STARS     OFFICIAL   TRUSTED
learn/tutorial                                                                 0
mzdaniel/buildbot-tutorial                                                     0
jbarbier/tutorial1                                                             0
odewahn/parallel_ml_tutorial                                                   0
modolo/redis                   Tutorial redis                                  0
mhubig/echo                    Simple echo loop from the tutorial.             0
ivarvong/redis                 From the redis tutorial. Just redis-server...   0
danlucraft/postgresql          Postgresql 9.3, on port 5432, un:docker, p...   0
amattn/postgresql-9.3.0        precise base, PostgreSQL 9.3.0 installed w...   0
```

## Dockerのイメージをダウンロードする  
```
$ docker pull learn/tutorial
Pulling repository learn/tutorial
8dbd9e392a96: Download complete
```

## Docker上でechoコマンドを実行する

```
$ docker run learn/tutorial echo "hello world"
hello world
```

pingをインストールしてみる

```
$ docker run learn/tutorial apt-get install -y ping
```

## イメージをコミット  

```
$ docker ps -l
CONTAINER ID        IMAGE                   COMMAND                CREATED             STATUS              PORTS               NAMES
0710132af978        learn/tutorial:latest   apt-get install -y p   5 minutes ago       Exit 0                                  naughty_poincare
$ docker commit 0710 learn/ping
535f88073c7d71ce1045293ebe62fccf01071e8afa56e5a6019938f25100d677
```

## Docerkの詳細の情報を見る  
```
$ docker ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
3f32f9b50c7c        learn/ping:latest   ping google.com     About a minute ago   Exit 1                                  agitated_wozniak
```

CONTAINER IDのみほしい場合以下のようにする。

```Bash
$ docker ps -l -q
3f32f9b50c7c
$ docker inspect 3f32f9b50c7c
```

## DockerFileの作成

## ビルドする

カレントディレクトリにDockerfileが存在する

```
$ sudo docker build -t oomatomo/base .
```

## 起動する 

```
$ docker run -d -t -i oomatomo/base　/bin/zsh
```

-d バックグランドで実行

## Dockerにアクセスする

```
$ sudo docker attach 'CONTAINER ID'
```

## kill

```
$ docker kill 'CONTAINER ID'
```


```
docker build -t base .
docker run -d --name b1 base
docker inspect --format ' {{ .NetworkSettings.IPAddress }} ' b1
```
