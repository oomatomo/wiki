# MySQL


## Install

## rpmからインストール

```
$ wget http://dev.mysql.com/get/Downloads/MySQL-5.5/MySQL-5.5.35-1.el6.x86_64.rpm-bundle.tar
$ tar xf MySQL-5.5.35-1.el6.x86_64.rpm-bundle.tar
$ yum localinstall ...
```

## ユーザー系
### root パスワード変更

```
# パスワード不要状態での起動
/usr/bin/mysqld_safe --user=root --skip-grant-tables & mysql mysql
# mysql 5.6ではパスワードは必須
> update user set Password=PASSWORD('ooma') where Host='localhost' and User='root';
```

### 外部アクセス

```
> GRANT ALL PRIVILEGES ON *.* TO root@'%' IDENTIFIED BY 'root のパスワード' WITH GRANT OPTION;
```

### ユーザ権限確認

```
> show grants for 'hoge'@'%';
```

## 設定ファイルの場所の確認

```
mysql --help | grep my.cnf
```

## Lock

```
SELECT GET_LOCK('test',3);
SELECT RELEASE_LOCK('test');
```

## Percona

#### pt-query-digest

ユーザ指定　クエリの実行時間の最大順

```
pt-query-digest --filter '($event->{user} || "") =~ m/hoge/' --order-by=Query_time:max
```

## Lock

有効にする

```
CREATE TABLE innodb_monitor (a INT) ENGINE=INNODB;
CREATE TABLE innodb_lock_monitor (a INT) ENGINE=INNODB;
```

無効にする

```
drop table innodb_monitor;
drop table innodb_lock_monitor;
```

## MysqlDump

```
# schemaのdump
mysqldump -uroot -d [データベース名]
# データのdump
mysqldump -uroot -q --single-transaction [データベース名]
```

## AUTO_INCREMENT初期化 

```
ALTER TABLE テーブル名 AUTO_INCREMENT = 1;
```

## csvをそのままINSERT

```
LOAD DATA INFILE "ファイル名" INTO TABLE テーブル名 FIELDS TERMINATED BY ',';
```

## unix timestamp
 
```
# date <- unixtimstamp 
from_unixtime(1445221897)
# unixtimstamp <- string
unix_timestamp('2015-10-01 00:00:00')
```
