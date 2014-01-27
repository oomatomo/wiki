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