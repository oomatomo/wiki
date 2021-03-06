Jenkins


# Jenkinsとは

# Jenkins Install
以下のコマンドでインストール可能である。
[jenkins install ](https://wiki.jenkins-ci.org/display/JENKINS/Installing+Jenkins+on+Red+Hat+distributions)

```
sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins-ci.org/redhat/jenkins.repo
sudo rpm --import http://pkg.jenkins-ci.org/redhat/jenkins-ci.org.key
sudo yum install jenkins
```

# Jenkins start

```
sudo service jenkins start
sudo chkconfig jenkins on
```

# プロキシ環境での対応

プロキシ環境以下でのJenkinsでは、プラグインのインストールができない

etc/ini.t/jenkinsでJAVA_CMDの設定の変更を行う

```
JAVA_CMD="$JENKINS_JAVA_CMD $JENKINS_JAVA_OPTIONS -DJENKINS_HOME=$JENKINS_HOME -Dhttp.proxyHost=hoge.net -Dhttp.proxyPort=8080 -jar $JENKINS_WAR"
```

# ssh の設定

```
sudo -u jenkins -H ssh-keygen -t rsa
```

# Plugin

* Git plugin
コーディング規約違反をチェックしてくれる
* Checkstyle Plugin
 コーディング規約違反をチェックしてくれる
* FindBugs Plugin
コンパイル後のバイトコードを解析してバグや不具合が発生しそうなコードをチェックしてくれる
* PMD Plugin
 バグや不具合が発生しそうなコードをチェックしてくれる
* DRY Plugin
コピペコードのような重複したコードをチェックしてくれる
* Warnings Plugin
コンパイラの警告をチェックしてくれる
* Analysis Collector Plugin
上記のpluginと依存関係があるため、これをインストールすると手間が省ける
* Clover Plugin
テストカバレッジを計測してくれる


# Checkstyle

必要なモジュール

* Perl::Metrics::Lite

```
measureperl-checkstyle --max_sub_lines 100 --max_sub_mccabe_complexity 10 --directory lib > checkstyle-result.xml
```

# Clover

必要なモジュール

* Storable
* Digest::MD5
* Devel::Cover
* Devel::Cover::Report::Clover::Builder
* Devel::Cover::Report::Clover
* TAP::Formatter::JUnit

```
cover -delete
cover -report html
cover -report clover
```

# Docker

```
# dockerのグループにjenkinsを追加する
# これでdockerが使える
usermod -a -G docker jenkins
# ついでにdocker-compose
curl -L https://github.com/docker/compose/releases/download/1.5.2/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```

volumes でやる場合は、権限に注意する
rootになってしまってjenkinsが書き込みできなくなる場合がある

# GitHub Pull Request Builder

```
# システム全体の設定
GitHub Server API URL: https://api.github.com
Credentials: トークンを作成
Application Setup: 空白でデフォルトの設定になる
# 各ジョブの設定
## ソースコード管理
Branch Specifier (blank for 'any'): ${sha1}
## ビルド・トリガ
GitHub API credentials: システム全体の設定で作成したトークンを利用
Admin list: ここに登録されたユーザからのコメントで操作可能になる
Use github hooks for build triggering: hookでbuildすることをonにする
Crontab line: hookをonしていたら、いらない
Trigger Setup: jobのトリガーごとにgithubにpushする
成功失敗ごとに設定できる
```

色々なところで設定するとかぶるので注意
