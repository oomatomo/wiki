AngularJS

Googleが作成したMVC

特徴
双方向のデータバインド
リアルタイム通信
モデルの値の変更に対してコントローラーが

使い方
スクリプトタグを埋め込む

<html lang='ja' ng-app>

コントローラーを定義する

Validateが豊富

invalid
novalid

## ng-class

簡単にToggleみたいなのが可能

http://stackoverflow.com/questions/15397252/angularjs-toggle-class-using-ng-class

```
<div div ng-class="fixContainer ? 'container' : 'container-fluid'" ></div>
```

```
$scope.fixContainer = 0;
```

上の場合は クラスは container-fluidが適応されるよ

## textでのchangeイベント

http://stackoverflow.com/questions/11868393/angularjs-inputtext-ngchange-fires-while-the-value-is-changing


## 豆知識

### Slimでのrequiredの書き方

Slimではrequiredの付与は以下のようになる

```
input#wikiname.form-control type='text' ng-model='wikiame' required=true
```
