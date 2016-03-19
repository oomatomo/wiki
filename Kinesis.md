# kinesis

## awscliを使った一通りの使い方

```
# ストリームの作成
aws kinesis create-stream --stream-name Foo --shard-count 1
aws kinesis describe-stream --stream-name Foo
# レコードを入れる
aws kinesis put-record --stream-name Foo --partition-key 123 --data testdata
# レコードの取得
aws kinesis get-shard-iterator --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON --stream-name Foo
SHARD_ITERATOR=$(aws kinesis get-shard-iterator --shard-id shardId-000000000000 --shard-iterator-type TRIM_HORIZON --stream-name Foo --query 'ShardIterator')\n
aws kinesis get-records --shard-iterator $SHARD_ITERATOR
# ストリームの削除
aws kinesis delete-stream --stream-name Foo
aws kinesis describe-stream --stream-name Foo
```
