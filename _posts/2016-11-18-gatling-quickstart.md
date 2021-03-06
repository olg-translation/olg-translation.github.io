---
layout: post
title:  "Gatling quick start docs"
date:   2016-11-18 19:00:00 +0900
categories: gatling
---
Gatling quick start

## Introduction

このセクションではシンプルなクラウドwebサーバで基本的なDSLを書いてロードテストをします。

## Getting the bundle

gatlingのzipは[ここ](http://gatling.io/#/resources/download)

## Installing

ダウンロードしたパッケージをunzipして好きなフォルダーにおいてください。

#### warning

windows users : Programsフォルダにガトリングを置いてしまうと、Permissionで問題がでる可能性があります。

gatlingを順番に実行しましょう。まずはJDKをインストールする必要があります。gatlingにはJDK8以上が必要ですが、JDKの最新バージョンを使用することを推奨しています。
OSのチューニングに関する全ての詳細は[Operations](http://gatling.io/docs/2.2.3/general/operations.html#operations)こちらを参照してください

#### warning

gatlingの起動スクリプトとmavenで起動する場合は ```JAVA_HOME``` をOSの環境変数に設定してください。
OSXのように独自のやり方でjavaの実行バージョンを確認することができる ``` java -version ```、なのでOSXを使用している場合は
異なのバージョンのjavaが実行されるかもしれません。もし ``` Unsupported major.minor version 51.0 ``` このエラーに遭遇したら
JDK8に対する ``` JAVA_HOME ``` を設定してください。


* In order 順番に

* regarding ~に関する

## ファイルエンコーディングに関して
gatlingはUTF-8が標準です。もし別のエンコーディングを使用したい場合は、

* レコーダーを使用している場合は、適切なエンコーディングを選んでください
* ```gatling.conf```で適切なエンコーディングを設定しシミュレーションのコンパイル時に使用され、
リクエストとレスポンスで実行されます。
* あなたのテキストエディターのエンコーディングが適切に設定されているか確認してください。

## Scalaコードに関して
gatlingシミュレーションは[Scala](http://www.scala-lang.org/)で記述します。だけど心配しないで。
Scalaの事を多く知らなくても基本的な機能を使う事ができます。

ほとんどの場合、gatlingのDSLであなたのやりたい事シミュレーションが作れます。  
もし、さらにScalaに興味があれば、[Twitter’s Scala School.](http://twitter.github.io/scala_school)をおすすめします。

### <span style="color: #ADD8E6; ">Note</span>

**このドキュメントを読んで**もしヘルプが必要であれば、   
無料で[Google Group](https://groups.google.com/forum/#!forum/gatling)に参加しましょう。

## Test Case

このページではgatlingのほぼすべての特徴を説明します。シミレーション、シナリオ、フィーダ、レコーダ、繰り返し処理 などなど

## お試しで使用するアプリケーション

このチュートリアルでは、Computer-Database という名前のアプリケーションを使います。 URL [http://computer-database.gatling.io.](http://computer-database.gatling.io)
このアプリケーションはシンプルなCRUD機能をもつコンピュータを管理するもので、このsampleは[Play Framework](https://www.playframework.com/) の バージョン2.3 で作られている。

## シナリオ

このアプリケーションでパフォーマンスのテストをするには、ユーザが操作する時に実際に何が起こるかを表すシナリオを作成しましょう。
実際のユーザがこのように操作すると考えています。

1.ユーザがアプリケーションに訪問する

2.macbookと検索する

3.関連する１つのモデルを見つけ、開く(addされているので複数見つかります。)

4.ホームに戻る

5.ページを反復する

6.新しいモデルを作る

## 基本

## Recorderを使う

シナリオの作成を簡単にするため、Gatlingに付属するツールであるRecorderを使用し、
Webアプリケーション上の操作を記録し、Gatlingシナリオとしてエクスポートすることができます。

レコーダーを起動するには、binディレクトリに配置してあるスクリプトを実行します。

* On Linux/Unix:

```
$GATLING_HOME/bin/recorder.sh
```

* On Windows:

```
%GATLING_HOME%\bin\recorder.bat
```

起動すると、GUIを使用してリクエストとレスポンスの設定できます。

### 起動オプション (画面参照)
* computerdatabase package
* BasicSimulation name
* Follow Redirects? checked
* Automatic Referers? checked
* Black list first filter strategy selected
* .*\.css, .*\.js and .*\.ico in the black list filters

![GUI](http://gatling.io/docs/2.2.2/_images/recorder1.png)

レコーダーを設定した後、ブラウザにGatlingRecorderのプロキシを設定し始めます。

### <span style="color: #ADD8E6; ">Note</span>

Recorderとブラウザ設定関する情報は、[Recorder reference page.](http://gatling.io/docs/2.2.2/http/recorder.html#recorder)を確認してください。

### Recorderを使用したシナリオの記録

アプリケーションをみてみましょう

1.'Search'という名前でタグ付け

2.このwebサイトに行く[http://computer-database.gatling.io](http://computer-database.gatling.io)

3.model名がmacbookなものを検索

4.Macbook proを選択

5.'Browse'という名前でタグ付け

6.ホームページに戻る

7.モデルページの'Next'ボタンを何度かクリック

8.'Edit'という名前でタグ付け

9.Add new computer をクリック

10.formを全て入力

11.Create this computer をクリック

実際のユーザとして行動するため、読む時間を取らずにすぐにページ遷移しないでください。
こうすることで、シナリオが実際のユーザに近い形で作成できます。

レコーダーのstopボタンをクリックすると、シナリオの作成が完了します。

作成したシナリオはgatlingをインストールしたディレクトリの ``` user-files/simulations/computerdatabase ```ディレクトリの中に ``` BasicSimulation.scala ```として保存されます

## Gatlingシナリオの説明

出力の内容は次の通り

```BasicSimulation.scala
package computerdatabase // 1

import io.gatling.core.Predef._ // 2
import io.gatling.http.Predef._
import scala.concurrent.duration._

class BasicSimulation extends Simulation { // 3

  val httpConf = http // 4
    .baseURL("http://computer-database.gatling.io") // 5
    .acceptHeader("text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8") // 6
    .doNotTrackHeader("1")
    .acceptLanguageHeader("en-US,en;q=0.5")
    .acceptEncodingHeader("gzip, deflate")
    .userAgentHeader("Mozilla/5.0 (Windows NT 5.1; rv:31.0) Gecko/20100101 Firefox/31.0")

  val scn = scenario("BasicSimulation") // 7
    .exec(http("request_1")  // 8
    .get("/")) // 9
    .pause(5) // 10

  setUp( // 11
    scn.inject(atOnceUsers(1)) // 12
  ).protocols(httpConf) // 13
}
```

コードはどういう意味でしょうか?

```
1. package宣言
2. 必要なクラスのimport
3. Simulationを継承したクラスを宣言
4. httpリクエストに関する共通の設定
```

### <span style="color: #ADD8E6; ">Note</span>

 ```val```はscalaの定数を表します。データ型宣言がなくてもscalaコンパイラが型推論を行います

```
5. baseURLメソッドで全てのurlリクエストに追加
6. ここで設定した共通のhttpヘッダーがすべてのリクエストに対して送信
7. シナリオの定義
8. httpの名前はrequest_1です。この名前はレポート出力で表示
9. このURLへgetリクエストを送信
10. 一時停止
```

### <span style="color: #ADD8E6; ">Note</span>

デフォルトでは秒で移ります。例えば ```pause(5)```は```pause(5 seconds)```と同等の意味です

```
11. シミュレーションを実行するのにシナリオを設定する場所
12. scnというシナリオに、一人のユーザーを注入する事の宣言
13. 上記で宣言したHTTP設定を割り当てる
```

### <span style="color: #ADD8E6; ">Note</span>
Simulationの構造に関して詳細を知りたければ、[Simulation reference page.](http://gatling.io/docs/2.2.3/general/simulation_structure.html#simulation-structure)を確認してください。

### Gatlingの実行

binディレクトリにある2つ目のスクリプトを起動します。

* On Linux/Unix:

 ```$GATLING_HOME/bin/gatling.sh```

* On Windows:

 ```%GATLING_HOME%\bin\gatling.bat```

simulation examplesの項目を見てください。

```scala
Choose a simulation number:
   [0] computerdatabase.BasicSimulation
```

シミュレーションが終わると、コンソールにhtmlレポートのリンクが表示されます。

### <span style="color: #ADD8E6; ">Note</span>

もしGatlingが動作しなければ、[FAQ](http://gatling.io/docs/2.2.3/project/faq.html#faq)か私たちの[Google Group.](https://groups.google.com/forum/#!forum/gatling)に尋ねてください。


## さらに進むには

進む準備が整ったら、[Advanced Tutorial](http://gatling.io/docs/2.2.3/advanced_tutorial.html#advanced-tutorial).を見てください。

