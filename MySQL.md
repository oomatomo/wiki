# MySQL


## Install

### rpmからインストール

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

## Percona

#### pt-query-digest

ユーザ指定　クエリの実行時間の最大順

```
pt-query-digest --filter '($event->{user} || "") =~ m/hoge/' --order-by=Query_time:max
```

## AUTO_INCREMENT初期化 

```
ALTER TABLE テーブル名 AUTO_INCREMENT = 1;
```

## csv

csvファイルのinsert
```
# csvファイルのinsert
LOAD DATA INFILE "ファイル名" INTO TABLE テーブル名 FIELDS TERMINATED BY ',';
# csvファイルの出力
select * from テーブル名
  into outfile 'ファイル名'
  fields terminated by ','
  optionally enclosed by '"';
  
# rdsの場合は使えないので注意
mysql -u username -p --database=dbname --host=rdshostname --port=rdsport --batch 
  -e "select * from yourtable" 
  | sed 's/\t/","/g;s/^/"/;s/$/"/;s/\n//g' > yourlocalfilename
```
https://stackoverflow.com/questions/9536224/exporting-table-from-amazon-rds-into-a-csv-file

## テーブルの状態

### テーブルの状態

```
SHOW TABLE STATUS LIKE '%_hoge';
```

### パーティション確認

```
SELECT TABLE_SCHEMA, TABLE_NAME, PARTITION_NAME, PARTITION_ORDINAL_POSITION, TABLE_ROWS
FROM INFORMATION_SCHEMA.PARTITIONS
WHERE TABLE_NAME LIKE '%_hoge';
```

## 日付系
### unix timestamp
 
```
# date <- unixtimstamp 
from_unixtime(1445221897)
# unixtimstamp <- string
unix_timestamp('2015-10-01 00:00:00')
```

### dateformat

```
DATE_FORMAT(start_time, '%d-%H')
```

## バックアップ
### mysqlDump

```
# schemaのdump
mysqldump -uroot -d [データベース名]
# データのdump
mysqldump -uroot -q --single-transaction [データベース名]
```

### xtrabackup

```
innobackupex --defaults-file=/etc/my.cnf --user=root --password=hoge --socket=/var/lib/mysql/mysql.sock --use-memory=4G /backup
# tar
innobackupex --defaults-file=/etc/my.cnf --user=root --password=hoge --socket=/var/lib/mysql/mysql.sock --use-memory=4G --stream=tar ./ |  gzip - > backup.tar.gz
```

slaveでのfullbackup

```

```

### 監視

### show processlist

接続元のIPを出力する

```
$ while true; do mysql -uhoge -phoge -e "show processlist;" | grep -o -P '\d+\.\d+\.\d+\.\d+' | sort | uniq ; sleep 1; clear; done;
```
