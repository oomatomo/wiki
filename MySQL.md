# MySQL


## Install

## rpmからインストール

```
$ wget http://dev.mysql.com/get/Downloads/MySQL-5.5/MySQL-5.5.35-1.el6.x86_64.rpm-bundle.tar
$ tar xf MySQL-5.5.35-1.el6.x86_64.rpm-bundle.tar
$ yum localinstall ...
```


## root パスワード変更

```
# パスワード不要状態での起動
/usr/bin/mysqld_safe --user=root --skip-grant-tables & mysql mysql
# mysql 5.6ではパスワードは必須
> update user set Password=PASSWORD('ooma') where Host='localhost' and User='root';
```

## 外部アクセス

```
> GRANT ALL PRIVILEGES ON *.* TO root@'%' IDENTIFIED BY 'root のパスワード' WITH GRANT OPTION;
```

## 設定ファイルの場所の確認

```
mysql --help | grep my.cnf
```

# Lock

```
SELECT GET_LOCK('test',3);
SELECT RELEASE_LOCK('test');
```

# Percona

## pt-query-digest

ユーザ指定　クエリの実行時間の最大順
pt-query-digest --filter '($event->{user} || "") =~ m/tachyon-m/' --order-by=Query_time:max
