# linux

### yum 

```Bash
$ rpm -Uhv http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
$ rpm -Uhv http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
```

### user

user add

-u userIDの指定  
-m --create-home ホームディレクトリの作成  
-s シェルの指定   

```Bash
$ useradd -u 1004 -m -s /bin/bash tomo
```

usermod

```Bash
# ログイン禁止
$ passwd -l tomo
$ usermod -s /sbin/nologin tomo
# ログイン許可
$ passwd -u tomo
$ usermod -s /bin/bash tomo
```

パスワード

```Bash
$ useradd -u 1004 -m -s /bin/bash tomo
```

リスト

```Bash
# user list
$ cat /etc/passwd | cut -d: -f1
# group list
$ cat /etc/group |cut -d: -f1
```

ログイン中のユーザ確認

```Bash
$ w
USER     TTY      FROM              LOGIN@   IDLE   JCPU   PCPU WHAT
vagrant  pts/0    192.168.137.1    Thu09    0.00s  0.26s  0.00s tmux -2
```

### netstat


### curl

-m 時間制限
-s 不要なデータを省く
-r 読み込むデータ量を指定

### Security
#### get checksum

```Bash
$ sudo yum install openssl
$ openssl dgst -sha256 tmux-1.8.tar.gz
SHA256(tmux-1.8.tar.gz)=f265401ca890f8223e09149fcea5abcd6dfe75d597ab106e172b01e9d0c9cd44
```

### ssh

```Bash
$ chmod 700 ~/.ssh
$ chmod 600 ~/.ssh/authorized_keys
```

sshからコマンド実行

ホスト名の後に実行したいコマンドを指定する

```Bash
$ ssh host ls -l
$ ssh host ls -l | grep test   ← ローカル側でgrep 実行
$ ssh host 'ls -l | grep test' ← リモート側でgrep 実行
```

### scp

-r ディレクトリの再帰的コピー
-v 詳細

```Bash
# ローカルからリモートへ
$ scp -r ./test tomo@test.com:/home/tomo

# リモートかローカルへ
$ scp -r tomo@test.com:/home/tomo /test
```

### tcpdump

```Bash
 sudo tcpdump -i eth1  -s 0 -l -A dst port 3306
```

### sudo

```Bash
# Passwordなし
ALL=(ALL) NOPASSWD:ALL
```

### 日付変更

`/usr/share/zoneinfo` にそれぞれのタイムゾーンのファイルがある。

/etc/localtimeとしてコピーすれば完了

```Bash
sudo rm -rf /etc/localtime
sudo cp -p /usr/share/zoneinfo/Asia/Tokyo /etc/localtime
```

### Memcached

#### バージョンチェック

ヘルプの一行目に表示される

```
$ memcached -h
memcached 1.4.5
```

netcatで調べる。

```
$ echo version | nc localhost 11211
VERSION 1.4.13
```

### iostat

```
yum install sysstat
```

### syslog-ng

```
yum install syslog-ng libdbi libdbi-drivers libdbi-dbd-mysql syslog-ng-libdbi
```

loggerで擬似的にログを送信できる　テストの時に便利

### crontab

#### ログ

crontab実行状況`/var/log/cron`に存在する

#### メール

yumでインストール mailx sendmail 
sendmailをstartさせる

```
yum install mailx sendmail
service sendmail start
```
crontab -eでMAILTO=メールアドレスで
設定されたメールアドレスに送信される
