Process & Thread

# 二つの違い
プロセスの中にスレッドがある。

# threadのスレッド
* create
生成 
* join
終了

# use thread
```perl
use thred;
my $thred = threads->create(¥&test);
$thread->join();
sub test {
}
```

# use thread::shared
共有変数

```perl
use threads::shared;
my $a :shared = 1;
```

## 競合の対処法

* lock
変数のみのロック

```perl
{
  lock($a);
}
```

* Thread::Semaphore
処理自体をロック

$sema->down;
$sema->up;

* Thread::Qu
キューを入れるスレッド
キューを取り出すスレッド

# Process



