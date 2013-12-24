# Mysql Replication

## 事前知識

### Binlog

Binary logを指す。バイナリ形式。
SQL文単位の変更内容が保存されている。
クエリの更新に要した時間　etc

###  RelayBinlog
マスターのbinlogと同期したファイル
バイナリ形式。

### I/O thread
マスターのDBのbinlogを読み込み、スレーブにRelayBinlogのファイルを作成する

### SQL thread
I/O thread で取得したRelayBinlogを元にスレーブに反映（SQL実行）させる

レプリケーションのときのみに作成される

## レプリケーションの構築手順

* マスター・スレーブの設定ファイル(my.cnf)変更
* マスター・スレーブの再起動
* マスター・スレーブにレプリケーションユーザの作成
* バックアップ作成( xtrabackup )
* スレーブスタート

## マスター・スレーブの設定ファイル(my.cnf)変更

### 必要最低限の設定

Master( 127.0.0.1 ) 
Slave  ( 127.0.0.3 )

```
# マスター
server_id=1
log_bin = mysql-bin

# スレーブ
server_id=3
```

server_idは被ってはいけないため色々な決め方がある。
IPアドレス4番目の値など。
3番目×256+4番目


### レプリケーション推奨設定

```
log_bin=mysql-bin
relay_log=relay-bin
sync_binlog =1        # トランザクションコミットされるたびにバイナリログの内容をディスクに同期する。ログイベントが失われない。
log_slave_updates= 1  # スレーブに複製されるイベントをスレーブのバイナリログに記録するため
skip_slave_start = 1  # スレーブの自動起動を阻止
```

### その他オプション

```
replicate_ignore_db = mysql # レプリケーション対象外のDB
read_only = 1             
expire_logs_days=7          # 定期的にrelaylbinlogの削除を行う

```

## マスター・スレーブにレプリケーションユーザの作成
GRANT REPLICATION SLAVE ON *.* TO 'hoge'@'127.0.0.1'; # Master
GRANT REPLICATION SLAVE ON *.* TO 'hoge'@'127.0.0.1'; # Slave


## バックアップ
xtrabackupのinnobackupexを利用します。
特徴、レプリケーション対象のDBがInnoDBであった場合書き込み止める必要がない。

## スレーブスタート

```
# マスター側でステータスの確認
> SHOW MASTER STATUS;

# server_id の確認に利用できる
> SHOW VARIABLES;

# スレーブ側でCHANGE MASTER文を実行
> CHANGE MASTER TO MASTER_HOST='127.0.0.1',
MASTER_USER='hoge',
MASTER_LOG_FILE='mysql-bin.00001',  # SHOW MASTER STATUSのFile
MASTER_LOG_POS=0;                             # SHOW MASTER STATUSのPosition

> START SLAVE; # レプリケーションを開始

> SHOW SLAVE STATUS \G; # スレーブの情報を表示

Read_Master_Log_Pos:  6864169
#スレーブが読み込んだマスターのbinlogの位置
Relay_Log_File:tachyon-sb-003-relay-bin.000059
# relay_logのファイル名
Relay_Log_Pos: 784377
# relay_logの中のイベントが完了した位置
Seconds_Behind_Master: Null以外
# マスターとスレーブの反映の差分　0だとスレーブにマスターのデータが完全反映された状態
Slave_IO_Runing: Yes
# マスターからスレーブへのbinlogの同期スレッドの有無
Slave_SQL_Runing:Yes
# 同期されたbinlogからSQLのスレーブを実行するためのスレッドの有無

> SHOW PROCESSLIST;   # 実行しているスレッドの確認を行う
#  Waiting for master to send event
#  Slave has read all relay log; waiting for the slave I/O thread to update it
#  それぞれI/OスレッドとSQLスレッドが存在するかと思われる
mysql> SHOW PROCESSLIST;
+----+-------------+-----------+------+---------+--------+-----------------------------------------------------------------------------+------------------+
| Id | User        | Host      | db   | Command | Time   | State                                                                       | Info             |
+----+-------------+-----------+------+---------+--------+-----------------------------------------------------------------------------+------------------+
|  1 | system user |           | NULL | Connect | 428690 | Waiting for master to send event                                            | NULL             |
|  2 | system user |           | NULL | Connect |    502 | Slave has read all relay log; waiting for the slave I/O thread to update it | NULL             |
| 10 | root        | localhost | NULL | Query   |      0 | NULL                                                                        | show processlist |
+----+-------------+-----------+------+---------+--------+-----------------------------------------------------------------------------+------------------+

```
##

## レプリケーションファイル

* master.info
 スレーブがマスターに接続するために必要な情報がある。

* relay-log.info
 スレーブに現在のバイナリログとリレーログの座標が含まれる

## スレーブからマスターへの昇格

```
# レプリケーションの停止
> STOP SLAVE
# レプリケーションに関するデータ削除
> RESET SLAVE ALL
```

## バイナリログの見方
Binlogはバイナリ形式だが、コマンドを利用して内容を確認できる。

