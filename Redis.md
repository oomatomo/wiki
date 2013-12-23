Redis

# Nosql


# データ型
## String
set key value
get key value

## List
rpush key value
lrange key 0 -1 (全て表示)

## Set
順不同なデータ、集合、タグ
sadd key value
smembers key
sunion key1 key2 和集合
sinter key1 key2 積集合

## Hash
連想配列
hset key label value
hget key labal
hmset key label value labal value
hmget key label label
hgetall key

## Sorted Set
zadd rank key value
zrange rank 0 -1
zrevrange rank 0 -1
zrank

## よく利用する関数
keys * 
exists
rename
del
expire

## 保存
bgsave　データ保存
shutdown redis-serverをシャットダウン

## setting
redis.conf
ログファイルの保存場所
ファイル名
スナップショットの感覚
（アクセス数の指定)
ポートの指定
