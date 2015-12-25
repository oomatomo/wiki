# linux

### OSバージョン確認

```
$ cat /etc/redhat-release 
CentOS release 5.8 (Final)
```

### カーネルバージョン確認

```
$ cat /proc/version
Linux version 2.6.32-431.el6.x86_64 (mockbuild@c6b8.bsys.dev.centos.org) (gcc version 4.4.7 20120313 (Red Hat 4.4.7-4) (GCC) ) #1 SMP Fri Nov 22 03:15:09 UTC 2013
$ uname -r
2.6.32-573.3.1.el6.x86_64
```

update 

```
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-6-6.el6.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-lt
```

  
### yum 

```Bash
$ rpm -Uhv http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
$ rpm -Uhv http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm
```
### spec

```
# OS
$ cat /etc/redhat-release
# カーネル
$ cat /proc/version
# CPU
$ cat /proc/cpuinfo
# メモリ
$ cat /proc/meminfo
```

### user

user add

-u userIDの指定  
-m --create-home ホームディレクトリの作成  
-s シェルの指定   

```Bash
$ useradd -m -s /bin/bash tomo
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

ユーザ情報の確認

```Bash
[root@vagrant-centos65 vagrant]# id
uid=0(root) gid=0(root) groups=0(root)
```

### LVM系

PV(Phygical Volume, 物理ボリューム)
VG(Volume Group, ボリュームグループ)
LV(Logical Volume, 論理ボリューム)

1. ディスクの確認
1. fdiskでLVMパーティション追加
1. 物理ボリュームを作成する
1. ボリュームグループを作成する
1. 論理ボリュームを作成
1. ファイルシステム変更する
1. ファイルをマウントする

#### ディスクの確認

```
# fdisk -l
・・・
Disk /dev/sdb: 85.9 GB, 85899345920 bytes
255 heads, 63 sectors/track, 10443 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0x00000000
```

新たに追加したディスクには、パーティションが無いことが分かります

#### fdisk で LVMパーティションの追加

n 新たなパーティション作成
t パーティションタイプ変更 (Linux LVM)
p パーティションの確認
w パーティションの保存

```
$ fdisk /dev/sdb

##############
Command (m for help): n
Command action
   e   extended
   p   primary partition (1-4)
p
Partition number (1-4): 3
First cylinder (1-10443, default 1):
Using default value 1
Last cylinder, +cylinders or +size{K,M,G} (1-10443, default 10443):
Using default value 10443

##############

Command (m for help): t
Selected partition 3
Hex code (type L to list codes): 8e
Changed system type of partition 3 to 8e (Linux LVM)

###############

Command (m for help): p

Disk /dev/sdb: 85.9 GB, 85899345920 bytes
255 heads, 63 sectors/track, 10443 cylinders
Units = cylinders of 16065 * 512 = 8225280 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk identifier: 0xbb410e4c

   Device Boot      Start         End      Blocks   Id  System
/dev/sdb3               1       10443    83883366   8e  Linux LVM

##############

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.

```

#### PVを作成する

```
# pvcreate /dev/sdb3
Physical volume "/dev/sdb3" successfully created
# pvdisplay
  "/dev/sdb3" is a new physical volume of "80.00 GiB"
  --- NEW Physical volume ---
  PV Name               /dev/sdb3
  VG Name
  PV Size               80.00 GiB
  Allocatable           NO
  PE Size               0
  Total PE              0
  Free PE               0
  Allocated PE          0
  PV UUID               OY7mrE-R1rC-mcGX-riAE-UXFe-q7e0-RL9l6V
```

#### VGを作成する

vg_sdbというVGが出来ました

```
# vgcreate vg_sdb /dev/sdb3
  Volume group "vg_sdb" successfully created
# vgdisplay
  --- Volume group ---
  VG Name               vg_sdb
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               80.00 GiB
  PE Size               4.00 MiB
  Total PE              20479
  Alloc PE / Size       0 / 0
  Free  PE / Size       20479 / 80.00 GiB
  VG UUID               4hfQX7-jpPQ-a0Yh-yyKd-WsJ5-UdTr-0BPTd4

```

#### LVを作成

ボリュームグループから必要な分だけのLVを作成します

lvcreate -n LV名 -L ボリュームサイズ VG名

今回は作成した75GをLVに当てます（ほぼ使い切る感じです）

```
# lvcreate -n lv_sdb_01 -L 75G vg_sdb
Logical volume "lv_sdb_01" created
# lvdisplay
  --- Logical volume ---
  LV Path                /dev/vg_sdb/lv_sdb_01
  LV Name                lv_sdb_01
  VG Name                vg_sdb
  LV UUID                056wP0-S6rx-P2gR-Jg4o-ivK7-gwSM-3ItYOv
  LV Write Access        read/write
  LV Creation host, time vagrant-centos65.vagrantup.com, 2014-08-14 02:28:27 +0000
  LV Status              available
  # open                 0
  LV Size                75.00 GiB
  Current LE             19200
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
    currently set to     256
  Block device           253:0
```

#### ファイルシステムの変更


```
mkfs -t ext4 /dev/vg_sdb/lv_sdb_01
mke2fs 1.41.12 (17-May-2010)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
4915200 inodes, 19660800 blocks
983040 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=4294967296
600 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632, 2654208,
        4096000, 7962624, 11239424

Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

This filesystem will be automatically checked every 28 mounts or
180 days, whichever comes first.  Use tune2fs -c or -i to override.

```

#### マウント

```
# mount /dev/vg_sdb/lv_sdb_01 /home
```

### IPv6

1. /etc/sysconfig/network  
 NETWORKING_IPV6=no

1. vi /etc/modprobe.d/disable-ipv6.conf  
 options ipv6 disable=1

1. chkconfig ip6tables off


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

### ssh-keygen

```
ssh-keygen -t rsa -C "ooma0301@gmail.com"
ssh-keygen -t rsa -f ファイル名
```

鍵の種類
-t rsa1
-t rsa
-t dsa

-C コメント
-f ファイル名

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

sshのログの場所
cat /var/log/secure

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

### iptables

```Bash
iptables -A OUTPUT -d <IPアドレス> -p all -j REJECT
service iptables save
```

### sudo

```Bash
# Passwordなし
ALL=(ALL) NOPASSWD:ALL
%wheel ALL=NOPASSWD: ALL
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

### logrotate

logrotateの実行

```
logrotate -f /etc/logrotate.d/httpd
```

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

### sort

IP系のsort
```
sort -n -t '.' -k 1,1 -k 2,2 -k 3,3 -k 4,4 | uniq 
```

### sambaをインストール 

```
yum -y install samba
chkconfig smb on
chkconfig nmb on
service smb start
service nmb start
smbpasswd -a vagrant
```

## unzip

-d で展開先のフォルダが指定出来る

```
unzip hoge.zip -d fuga
```

## sed

先頭と末尾に”を追加する

```
sed 's/^\(.\+\)$/\"\1\"/g'
```

## Postfix

```
# キューの確認
mailq
# メールキューを削除
postsuper -d ALL
```

設定ファイルの位置
config: /etc/postfix/main.cf
config: /etc/postfix/master.cf

## Sendmail

キューがある位置
/var/spool/mqueue

```
# キュー削除
rm -f /var/spool/mqueue/*
```

## rm 

```
rm -f ./*
bash: /bin/rm: Argument list too long
# 正規表現で指定したファイル数が多いと出るエラー
find . -type f -print | xargs rm
```
