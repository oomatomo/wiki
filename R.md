
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
z <- as.POSIXct(strptime("2015-1-30 15:00:00","%Y-%m-%d %H:%M:%S"))
# testのdatetimeのカラムの14:01:00～14:02:00のデータを取得してくれる
test <- subset(test, datetime >= x & z >= datetime )
```

### ggplot2

### グラフの種類

* geom_line():折れ線
* geom_point():散布図

### 目盛りの操作

* xlab(""):X軸の目盛り
* ylab(""):Y軸の目盛り
* ggtitle(""):グラフのタイトル
* xlim(c(0, 100)):X軸の表示範囲
* ylim(c(0, 100)):Y軸の表示範囲

### その他

* interaction(column): グループ化の時に利用する 
* theme_bw(base_family = "HiraKakuProN-W3"): 文字化け対策

#### 複数グラフ表示

```
multiplot <- function(..., plotlist=NULL, file, cols=1, layout=NULL) {
  require(grid)

  # Make a list from the ... arguments and plotlist
  plots <- c(list(...), plotlist)

  numPlots = length(plots)

  # If layout is NULL, then use 'cols' to determine layout
  if (is.null(layout)) {
    # Make the panel
    # ncol: Number of columns of plots
    # nrow: Number of rows needed, calculated from # of cols
    layout <- matrix(seq(1, cols * ceiling(numPlots/cols)),
                    ncol = cols, nrow = ceiling(numPlots/cols))
  }

 if (numPlots==1) {
    print(plots[[1]])

  } else {
    # Set up the page
    grid.newpage()
    pushViewport(viewport(layout = grid.layout(nrow(layout), ncol(layout))))

    # Make each plot, in the correct location
    for (i in 1:numPlots) {
      # Get the i,j matrix positions of the regions that contain this subplot
      matchidx <- as.data.frame(which(layout == i, arr.ind = TRUE))

      print(plots[[i]], vp = viewport(layout.pos.row = matchidx$row,
                                      layout.pos.col = matchidx$col))
    }
  }
}
```

### ggplotのオブジェクトのリスト化

```
graph_list <- list()
for(type in unique(data$V3) ) {
  p <- ggplot(data=subset(data, V3 == type), aes( x = V1) ) + 
  geom_point(aes(y=V2, label=V3), colour="#0072B2", size=0.1) +
  graph_list[[length(graph_list) + 1]] <- p
}

multiplot(plotlist=graph_list, cols=2)
```

### ggplotでの折れ線グラフにメモリごとにラベルを追加する

```
ggplot(data=subset(data, V3 == type) + 
geom_line() +
geom_text(aes(label= hoge, size=0.003, hjust=1, vjust=5)) 
```

labal：ラベルに表示するためのカラム  
size ：ラベルのフォントサイズ  
hjust：横の位置の数字  
vjust：縦の位置の数字  

## グループ化

### distinct
重複する行を削除する

```{r}
data %>%
  distinct(重複したいカラム) %>%
  select(取得するカラム)
```

### group_by

mean = 平均

summarise_each ですべてのカラムに適応させる

summarise(mean=mean(カラム))でカラムだけの平均が出せる

```{r}
library(dplyr)
 datas %>%
+ group_by(カラム１, カラム2) %>%
+ summarise_each(funs(mean(., na.rm = TRUE)))
```

### Join

data frameのxとyをjoinする
カラム a, b, cでjoinする

```{r}
library(dplyr)
inner_join(x, y, by=c("a", "b", "c"))

```

### aggregate

mean = 平均
```
aggregate(. ~ カラム名, data=data, FUN=mean, na.rm=TRUE) 
aggregate(. ~ カラム1+カラム2, data=web_result, mean, na.rm=TRUE)
```

### unique

ユニークな値がほしいとき

```
ddd <- c("11","12","11")
data <- data.frame(ddd)
unique(data)
```

### plyr

#### count

count(データ,c(列))

列ごとのカウントを出してくれる

```
ddd <- c("11","12","11")
eee <- c("300","200","100")
data <- data.frame(ddd,eee)
count(data, c('ddd'))
```

## Rstdio

### 文字化け

Tools -> General -> Default text encoding
デフォルト [Ask] -> [UTF-8]


