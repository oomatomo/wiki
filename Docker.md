# Docker

## 公式が一番

[公式](http://docs.docker.io/en/latest/installation/windows/)

## 準備

```
git clone https://github.com/dotcloud/docker.git
cd docker
vagrant up
```

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

```
$ sudo docker build -t oomatomo/base .
```

## 起動する 

```
$ docker run -d -t -i oomatomo/base　/bin/zsh
```

## Dockerにアクセスする

```
$ sudo docker attach 'CONTAINER ID'
```

