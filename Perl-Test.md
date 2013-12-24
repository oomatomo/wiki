# About Test of Perl

## Testについて
テストコードのファイルは`test.t`と拡張子が `.t`となっている。

`subtest`を用いてテストコードを書く。

```perl

subtest 'do test' => sub {
  # テストの内容
};

subtest 'do test' => sub {
  # テストの内容
  subtest 'andriod' => sub {
    # テストの内容
  };
  subtest 'ios' => sub {
    # テストの内容
  };
};
done_testing();
```

## 利用するモジュール

### Test::More
テストコードの大部分で利用するモジュール。
[metacpanはこちら](https://metacpan.org/module/Test::More)

```perl

use Test::More;

# 条件が真
ok( '条件', 'テストメッセージ' );

# A == B
is( '値A', '値B', 'テストメッセージ' );

# A != B
isnt( '値A', '値B', 'テストメッセージ' );

# ハッシュ or 配列 A ==B
is_deeply( '値A', '値B', 'テストメッセージ' );

# log
diag( 'ログの内容' );

```

### Test::mysqld
テスト用のデータベースを立ててくれる。
[metacpanはこちら](https://metacpan.org/module/Test::mysqld)

```perl

# AppDriverの場合
use Test::Luxon::mysqld;

my $mysqld = Test::Luxon::mysqld->setup;
Test::Luxon::mysqld->cleanup( $mysqld );


# 通常の場合
my $mysqld = Test::mysqld->new(
      my_cnf => {
          'port' => 3307,
      },
      mysqld           => '/usr/sbin/mysqld',
      mysql_install_db => '/usr/bin/mysql_install_db',
  )  or die $Test::mysqld::errstr;

# mysqlへ接続 
my $dbh = DBI->connect( $mysqld->dsn, 'root',  {
        AutoCommit => 0,
        PrintError => 1,
        RaiseError => 1,
        mysql_enable_utf8 => 1,
    } );

# do DLL(データ定義言語)
$dbh->do( "DLL" );

```

### Test::mysqld

DBの情報をymlへ変換

```perl

use DBI;
use File::Temp qw(tempfile);
use Test::More;
use Test::Fixture::DBI;
 
my ( undef, $filename ) = tempfile;
my $dbh = DBI->connect( "dbi:SQLite:dbname=$filename", "", "" );
 
# DBの生成情報（CREATE文)
construct_database(
  dbh => $dbh,
  database => '/path/to/schema.yaml',
);
 
# DBのデータ情報
construct_fixture(
  dbh => $dbh,
  fixture => '/path/to/fixture.yaml',
);

```
### Test::WWW::Mechanize::Catalyst
テスト用にCatalystを立ててくれる。
[metacpanはこちら](https://metacpan.org/module/Test::WWW::Mechanize::Catalyst)

```perl

use Test::WWW::Mechanize::Catalyst;
my $mech = Test::WWW::Mechanize::Catalyst->new( catalyst_app => 'アプリ(Tachyon::Website)' );

# do not redirect
$mech->requests_redirectable( [] );

subtest 'check response' => sub {
  # URLへアクセス
  my $res = $mech->get( 'URL' );
  # レスポンスのチェック
  is ( $res->code, 404, 'page not found' );
  # レスポンスのテキストチェック
  $mech->text_contains('An error has occurred.(40001)');
};

# change user agent
# for andriod and ios
$mech->agent( '変更したいユーザー情報' );

```

### Test::Memcached

テスト用にMemcachedを立ててくれる。
[metacpanはこちら](https://metacpan.org/pod/Test::Memcached)

```perl

my $memd = Test::Memcached->new();
# Memcachedの起動 
$memd->start;
# 起動したMemcachedのポートを取得
my $port = $memd->option('tcp_port');
# 通常のCache::Memcached::Fastを利用する。ポートのみ渡せばよい。
return Cache::Memcached::Fast->new( { servers => ["127.0.0.1:$port"] } ); 

```

## prove　オプションについて
proveとはテストコード実行のためのコマンドである。
proveは数多くのオプションをもっている。
[詳しくはこちらを](http://perl-users.jp/articles/advent-calendar/2011/test/21)

```
# prove -h
     -v,  --verbose         Print all test lines.
     -l,  --lib             Add 'lib' to the path for your tests (-Ilib).
     -c,  --color           Colored test output (default).
     -D   --dry             Dry run. Show test that would have run.
     -r,  --recurse         Recursively descend into directories.
     -j,  --jobs N          Run N test jobs in parallel (try 9.)
          --state=opts      Control prove's persistent state.
```
オプション「 -v 」のとき

```
# prove -v
t/controller/supplier/Party.t ..
        ok 1 - response code is 302, because has web_session cookie.
        ok 2 - click is exists
        ok 3 - redirect click param is OK
        1..3
    ok 1 - android
        ok 1 - response code is 302, because has web_session cookie.
        ok 2 - click is exists
        ok 3 - redirect click param is OK
        1..3
    ok 2 - ios
    1..2
ok 1 - click
    ok 1 - achieve request is OK
    ok 2 - achieve ok
    1..2
ok 2 - achieve
1..2
ok
All tests successful.
Files=1, Tests=2, 10 wallclock secs ( 0.00 usr  0.02 sys +  5.35 cusr  0.59 csys =  5.96 CPU)
Result: PASS

```
オプション「 --state 」のとき

```
# prove --state=save
テストの結果が「.prove」に保存される。
# prove --state=failed
失敗したテストのみ実行
# prove --state=all
すべてのテストの実行

```

