
## R

### CSV

read.csv
write.csv

### DateTimeのオブジェクト

test$datetimeが '13:10:10' だった場合有効,ただし日付は実行した日になる
やっていることは、文字列からDateTime型に変換している

```{r}
test$datetime <- as.POSIXct(strptime(test$datetime, "%H:%M:%S"))
```

### dataの絞り込み

```
x <- as.POSIXct(strptime("14:01:00","%H:%M:%S"))
y <- as.POSIXct(strptime("14:02:00","%H:%M:%S"))

# testのdatetimeのカラムの14:01:00～14:02:00のデータを取得してくれる
test <- subset(test, datetime >= x & z >= datetime )
```

### ggplot2

#### 折れ線グラフ

```
ggplot(data=test,aes(x=test$date)) +
    geom_line(aes(y = test$diff, color="diff")) +
    geom_line(aes(y = test$count, color="count")) +
    xlab("Time") + ylab("count") +  ggtitle("Test")
```

