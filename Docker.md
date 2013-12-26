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

## start Docker

```
Usage: docker [OPTIONS] COMMAND [arg...]
 -H=[unix:///var/run/docker.sock]: tcp://host:port to bind/connect to or unix://path/to/socket to use

A self-sufficient runtime for linux containers.

Commands:
    attach    Attach to a running container
    build     Build a container from a Dockerfile
    commit    Create a new image from a container's changes
    cp        Copy files/folders from the containers filesystem to the host path
    diff      Inspect changes on a container's filesystem
    events    Get real time events from the server
    export    Stream the contents of a container as a tar archive
    history   Show the history of an image
    images    List images
    import    Create a new filesystem image from the contents of a tarball
    info      Display system-wide information
    insert    Insert a file in an image
    inspect   Return low-level information on a container
    kill      Kill a running container
    load      Load an image from a tar archive
    login     Register or Login to the docker registry server
    logs      Fetch the logs of a container
    port      Lookup the public-facing port which is NAT-ed to PRIVATE_PORT
    ps        List containers
    pull      Pull an image or a repository from the docker registry server
    push      Push an image or a repository to the docker registry server
    restart   Restart a running container
    rm        Remove one or more containers
    rmi       Remove one or more images
    run       Run a command in a new container
    save      Save an image to a tar archive
    search    Search for an image in the docker index
    start     Start a stopped container
    stop      Stop a running container
    tag       Tag an image into a repository
    top       Lookup the running processes of a container
    version   Show the docker version information
    wait      Block until a container stops, then print its exit code

```

## Dockerのバージョンを確認  
```
?  ~  docker version
Client version: 0.7.2
Go version (client): go1.2
Git commit (client): 28b162e
Server version: 0.7.2
Git commit (server): 28b162e
Go version (server): go1.2
Last stable version: 0.7.2
```

## Docker イメージを検索する  
```
?  ~  docker search tutorial
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
?  ~  docker pull learn/tutorial
Pulling repository learn/tutorial
8dbd9e392a96: Download complete
```  

## Docker上でechoコマンドを実行する

```
?  ~  docker run learn/tutorial echo "hello world"
hello world
```

```
?  ~  docker run learn/tutorial apt-get install -y ping
Reading package lists...
Building dependency tree...
The following NEW packages will be installed:
  iputils-ping
0 upgraded, 1 newly installed, 0 to remove and 0 not upgraded.
Need to get 56.1 kB of archives.
After this operation, 143 kB of additional disk space will be used.
Get:1 http://archive.ubuntu.com/ubuntu/ precise/main iputils-ping amd64 3:20101006-1ubuntu1 [56.1 kB]
debconf: delaying package configuration, since apt-utils is not installed
Fetched 56.1 kB in 0s (69.6 kB/s)
Selecting previously unselected package iputils-ping.
(Reading database ... 7545 files and directories currently installed.)
Unpacking iputils-ping (from .../iputils-ping_3%3a20101006-1ubuntu1_amd64.deb) ...
Setting up iputils-ping (3:20101006-1ubuntu1) ...
```

## イメージをコミット  

```
?  ~  docker ps -l
CONTAINER ID        IMAGE                   COMMAND                CREATED             STATUS              PORTS               NAMES
0710132af978        learn/tutorial:latest   apt-get install -y p   5 minutes ago       Exit 0                                  naughty_poincare
?  ~  docker commit 0710 learn/ping
535f88073c7d71ce1045293ebe62fccf01071e8afa56e5a6019938f25100d677
```

## Docerkの詳細の情報を見る  
```
? ~ docker ps -l
CONTAINER ID        IMAGE               COMMAND             CREATED              STATUS              PORTS               NAMES
3f32f9b50c7c        learn/ping:latest   ping google.com     About a minute ago   Exit 1                                  agitated_wozniak
```

CONTAINER IDのみほしい場合以下のようにする。

```
? ~ docker ps -l -q
3f32f9b50c7c
```

```
?  ~  docker inspect 3f3
[{
    "ID": "3f32f9b50c7ca75ca3efdb78d9bd8291a5b5ce1c666cd6e02b889f1d083ca09b",
    "Created": "2013-12-23T09:56:42.372220573Z",
    "Path": "ping",
    "Args": [
        "google.com"
    ],
    "Config": {
        "Hostname": "3f32f9b50c7c",
        "Domainname": "",
        "User": "",
        "Memory": 0,
        "MemorySwap": 0,
        "CpuShares": 0,
        "AttachStdin": false,
        "AttachStdout": true,
        "AttachStderr": true,
        "PortSpecs": null,
        "ExposedPorts": null,
        "Tty": false,
        "OpenStdin": false,
        "StdinOnce": false,
        "Env": null,
        "Cmd": [
            "ping",
            "google.com"
        ],
        "Dns": null,
        "Image": "learn/ping",
        "Volumes": null,
        "VolumesFrom": "",
        "WorkingDir": "",
        "Entrypoint": null,
        "NetworkDisabled": false
    },
    "State": {
        "Running": false,
        "Pid": 0,
        "ExitCode": 1,
        "StartedAt": "2013-12-23T09:56:42.37846083Z",
        "FinishedAt": "2013-12-23T09:57:30.576440818Z",
        "Ghost": false
    },
    "Image": "535f88073c7d71ce1045293ebe62fccf01071e8afa56e5a6019938f25100d677",
    "NetworkSettings": {
        "IPAddress": "",
        "IPPrefixLen": 0,
        "Gateway": "",
        "Bridge": "",
        "PortMapping": null,
        "Ports": null
    },
    "SysInitPath": "/usr/bin/docker",
    "ResolvConfPath": "/etc/resolv.conf",
    "HostnamePath": "/var/lib/docker/containers/3f32f9b50c7ca75ca3efdb78d9bd8291a5b5ce1c666cd6e02b889f1d083ca09b/hostname",
    "HostsPath": "/var/lib/docker/containers/3f32f9b50c7ca75ca3efdb78d9bd8291a5b5ce1c666cd6e02b889f1d083ca09b/hosts",
    "Name": "/agitated_wozniak",
    "Driver": "aufs",
    "Volumes": {},
    "VolumesRW": {},
    "HostConfig": {
        "Binds": null,
        "ContainerIDFile": "",
        "LxcConf": [],
        "Privileged": false,
        "PortBindings": {},
        "Links": null,
        "PublishAllPorts": false
    }
}]%
```

## DockerFileの作成

## ビルドする

```
sudo docker build -t oomatomo/base .
```

## 起動する 

```
docker run -d -t -i oomatomo/base　/bin/zsh
```

## Dockerにアクセスする

```
sudo docker attach 'CONTAINER ID'
```

