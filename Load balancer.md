Load balancer

## ロードバランサ
負荷分散装置
外部からの要求を一元的に管理し複数のサーバに要求に転送する装置
SSL通信の処理を行う
LBの性能はSSLの処理能力で決まる

## LBの仕組み　内部→外部
NATでIPの変換を行う
SNATは多対１
外部との通信は、LBだけでなく

## LBの仕組み　外部→内部

サーバ群を同じグループをpoolという

## LBの配分

Round Robin
均等に負荷分散

Ratio
サーバごとに重みを設定し、割合に応じた負荷分散。

Least Connections 
現在の」コネクション数が最も小さいサーバ転送

Fastest
Layer7の未処理数が少ないサーバへ負荷分散

Leas Session
セッションが少ないサーバへ負荷分散

Observed　よく使う設定
過去の不可比率をもとに負荷分散

## ヘルスモニター

http
 get /
 webサーバのトップページが開けるかの判断を行っている

## 設定属性

Ratio 負担

poolのless thanでいくつまで死んでよいか

## SSLの証明書

BIG-IPではVirtual ServerがSSL証明書を処理してくれる
サーバへは暗号化が解ける
LBで処理されたSSL通信はヘッダーに追加情報が追加される

## iRules
LB内のスクリプトみたいなもの
URLの組み替え

## DBの死活など
LBはあくまでも
DBの監視はCatic,Nagios
ping
ssh

## SSL証明書について
SSL証明書は３枚ある
key cer csr
LBで必要なのはkey,cerとなる
