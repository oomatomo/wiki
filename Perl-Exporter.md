# About Perl Exporter

今回、仕事の開発でExporterを利用しての開発を行いました。
その際に先輩にExporterの便利さに感動したので簡単にメモを残します。
今まで知らなかったのが恥ずかしかったです。

Exporterはこちら(metacpan)

```perl
 use Encode qw(encode);  
```

Perlで上のuseの使い方を見かけることがあると思います。  

これはEncodeクラスのなかでencodeというメソッドを利用することを宣言しています。  

①　@EXPORT  
　該当するクラスをuseすると使用できる。

②　@EXPORT_OK  
　該当するクラスをuseのみでは使用できない。  
　上記のように   
　　use モジュール名 qw( 使用したいモジュール名 );  

③　%EXPORT_TAGS  
　 タグですね。複数のメソッドをグループ化し、一括で読み込むことが出来ます。  

詳しくはmetacpanを見ていただけると分かると思います。  

どんな時に使うか？  

あくまで自分が活用した例です。  

会社では Perl + Catalystで開発をしています。  
今回、Schema(DBIx::Class::Schema)やMemcached(Cache::Memcached::Fast)を全て  
Role化したら楽じゃねと思いRole化に取り掛かりました。  

```perl
package MyApp::API;

with 'MyApp::Role::Schema', 'MyApp::Role::Memcached';

sub ...
```

お。出来たと思ったのですが、そうでもなかったです。  

実は上記の二つのロールのほかに別のロールも作成してました。   
それは「MyApp::Role::Config」です。  
Catalystの設定(ymlファイル)を読み込みハッシュリファレンスを返す役割を担ってます。  
SchemaやMemcachedはこの設定を元にインスタンスを生成してるからです。  
さらに MyApp::Role::SchemaとMyApp::Role::Memcahedでwithしてました。  

```perl
package MyApp::Role::Schema;

with 'MyApp::Role::Config';

sub ...

package MyApp::Role::Memcached;

with 'MyApp::Role::Config';

sub ...
```
 
ここで問題となるのがオブジェクト指向での菱形継承問題です。   
ただ今回の使い方はRoleなので、厳密には菱形継承問題にはならないかもしれません。  
しかし、なんか変なので変更することにしました。  

この解決のためにExporterを利用しました。  

さらにBase.pmを作成して、API側ではBase.pmを継承することにしています。  

```perl
package MyApp::Base;

use MyApp::Bootstrap qw ( get_schema get_memcached );

has 'schema' => {
   ・・・
   lazy_build => 1,
); 

sub _build_schema {
  return get_schema;
} 
```

 メリット  
- 必要なときに簡単に呼び出せる。   
- 便利な関数を一つに集約できる点。   

これがあるので凄いと思いました。  

仕組みはとても簡単で。  
perlのモジュールをuseする際必ず呼ばれる関数でimportが存在します。  
今回のExporter.pmでのソースがほとんどがimport内で処理が書かれています。  
