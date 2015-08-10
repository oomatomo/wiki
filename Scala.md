
# Scala

## Install

brew install scala

#### play install

brew install typesafe-activator

##### brew install play

Play 2.3 からはリプレースされている

Error: No available formula for play
Play 2.3 replaces the play command with activator:
  brew install typesafe-activator

You can read more about this change at:
  http://www.playframework.com/documentation/2.3.x/Migration23
  http://www.playframework.com/documentation/2.3.x/Highlights23
  
## activator

### Error

#### newのときにタイムアウトになる

https://github.com/typesafehub/activator/issues/728

```
ava.util.concurrent.TimeoutException: Futures timed out after [5 seconds]
	at scala.concurrent.impl.Promise$DefaultPromise.ready(Promise.scala:219)
	at scala.concurrent.impl.Promise$DefaultPromise.result(Promise.scala:223)
	at scala.concurrent.Await$$anonfun$result$1.apply(package.scala:111)
	at scala.concurrent.BlockContext$DefaultBlockContext$.blockOn(BlockContext.scala:53)
	at scala.concurrent.Await$.result(package.scala:111)
	at activator.ActivatorCli$.activator$ActivatorCli$$generateProjectTemplate(ActivatorCli.scala:147)
	at activator.ActivatorCli$$anonfun$apply$1$$anonfun$13$$anonfun$apply$3.apply(ActivatorCli.scala:105)
	at activator.ActivatorCli$$anonfun$apply$1$$anonfun$13$$anonfun$apply$3.apply(ActivatorCli.scala:104)
	at scala.Option.map(Option.scala:145)
	at activator.ActivatorCli$$anonfun$apply$1$$anonfun$13.apply(ActivatorCli.scala:104)
	at activator.ActivatorCli$$anonfun$apply$1$$anonfun$13.apply(ActivatorCli.scala:103)
	at scala.Option.flatMap(Option.scala:170)
	at activator.ActivatorCli$$anonfun$apply$1.apply$mcI$sp(ActivatorCli.scala:103)
	at activator.ActivatorCli$$anonfun$apply$1.apply(ActivatorCli.scala:19)
	at activator.ActivatorCli$$anonfun$apply$1.apply(ActivatorCli.scala:19)
	at activator.ActivatorCli$.withContextClassloader(ActivatorCli.scala:179)
	at activator.ActivatorCli$.apply(ActivatorCli.scala:19)
	at activator.ActivatorLauncher.run(ActivatorLauncher.scala:35)
	at xsbt.boot.Launch$$anonfun$run$1.apply(Launch.scala:109)
	at xsbt.boot.Launch$.withContextLoader(Launch.scala:129)
	at xsbt.boot.Launch$.run(Launch.scala:109)
	at xsbt.boot.Launch$$anonfun$apply$1.apply(Launch.scala:36)
	at xsbt.boot.Launch$.launch(Launch.scala:117)
	at xsbt.boot.Launch$.apply(Launch.scala:19)
	at xsbt.boot.Boot$.runImpl(Boot.scala:44)
	at xsbt.boot.Boot$.main(Boot.scala:20)
	at xsbt.boot.Boot.main(Boot.scala)
```

activator -Dactivator.timeout=30s new web

-Dでタイムアウトを長くする


### コンソール

activator　でコンソールコマンドに移動できる

dependencies
　buid.sbtの設定を元に依存関係のライブラリをインストールする
~compile
　ファイルが変更するたびにコンパイルする
　

### 環境ごとの設定ファイル

include で共通の設定ファイルを読み込むことができる


# giter8

## install 

```
brew install giter8
```

テンプレートの一覧
https://github.com/n8han/giter8/wiki/giter8-templates

Scalaのプロジェクトのテンプレートを作成する

```
g8 pmandera/basic-scala-project
```

pmandera/basic-scala-project　はテストのフレームワークにspecs2を使ってる



# For yield

```scala
for {
  a <- Option(1)
  b <- Option(2)
} yield ( a, b ) match {
  case (a, b) =>
    a + b
}
```

```scala
( for {
  a <- Option(1)
  b <- Option(2)
} yield ( a, b ) ) match {
  case Some(a, b) =>
    a + b
  case None =>
    None
}
```

# List Index

```scala
val list = List("a", "b", "c")
list.zipWithIndex.foreach( v => println(v._1, v._2) )

list.zipWithIndex.map( v => v._2 -> v._1 ).toMap
scala.collection.immutable.Map[Int,String] = Map(0 -> a, 1 -> b, 2 -> c)

val array = Array("a", "b", "c")
list.zipWithIndex.foreach( case(e:String, i:Int) => println(i, e) )
```

# sbt native package

マルチプロジェクトでひとつにまとめる

target/universal/*.zipに出来る

```scala
import com.typesafe.sbt.SbtNativePackager.packageArchetype
import NativePackagerHelper._
// confを含む
mappings in Universal ++= directory("conf")

val releaseSetting = packageArchetype.java_server ++ Seq(
    maintainer := "test",
    packageDescription := "test Scala Application",
    packageSummary := "test Application",
    // entrypoint
    // bin/..で実行するもの
    mainClass in Compile := Some("test.cli.Main")
)

lazy val root = (project in file("."))
    .aggregate(cli, lib)
    .dependsOn(cli, lib)
    .settings(releaseSetting: _*)
```


