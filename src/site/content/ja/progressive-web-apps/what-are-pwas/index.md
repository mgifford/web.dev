---
layout: post
title: プログレッシブウェブアプリとは何ですか？
authors:
  - samrichard
  - petelepage
date: 2020-01-06
updated: 2020-02-24
description: |2-

  プログレッシブウェブアプリ（PWA）とそれを他のウェブアプリから差別化する 3 つの柱についてご紹介します。
tags:
  - progressive-web-apps
---

ウェブは素晴らしいプラットフォームです。デバイスやオペレーティングシステム全体でのユビキタス性の組み合わせ、ユーザー中心のセキュリティモデル、および仕様も実装も単一の企業が制御しているものではないという事実があるため、Webはソフトウェア開発のユニークなプラットフォームとなっています。固有のリンク機能と組み合わせることで、検索して見つけたものを誰とでも、どこからでも共有することができます。あなたがウェブサイトにアクセスするときはいつでも、最新の状態にあり、そのサイトでの経験はあなたの望みどおりにその時だけのものにも、永続的なものにもなります。 ウェブアプリケーションは、単一のコードベースで、*場所やデバイスを問わずあらゆるユーザー*に達することができます。

プラットフォーム固有のアプリケーションは、非常に豊富で信頼性が高いことで知られています。それらは、ホーム画面、ドック、およびタスクバーに絶えず存在しています。これらは、ネットワーク接続に関係なく機能します。こうしたアプリケーションは、独自のスタンドアロン体験として起動します。ローカルファイルシステムからファイルを読み書きしたり、USB、シリアル、Bluetooth経由で接続されたハードウェアにアクセスしたり、連絡先やカレンダーイベントなどのデバイスに保存されているデータとやり取りしたりすることもできます。これらのアプリケーションでは、写真を撮ったり、ホーム画面に表示されている再生中のソングを確認したり、別のアプリを使用中にソングの再生を制御したりできます。プラットフォーム固有のアプリケーションは、まるでそれらが実行されるデバイスの*一部*であるかのように感じられます。

<figure class="w-figure">{% Img src="image/tcFciHGuF3MxnTr1y5ue01OGLBn2/1DKtUFjXLJbiiruKA9P1.svg", alt="高機能を持つプラットフォーム固有のアプリ、高リーチを持つウェブアプリ、高機能と高リーチの両方を持つプログレッシブウェブアプリの相対的機能とリーチを示すグラフ。", width="370", height="367" %}<figcaption class="w-figcaption w-figcaption--fullbleed">プラットフォーム固有のアプリ、ウェブアプリ、プログレッシブウェブアプリの機能およびリーチの比較。</figcaption></figure>

プラットフォーム固有のアプリとウェブアプリを機能とリーチの観点から考えると、プラットフォーム固有のアプリは機能に優れていることを示す一方で、ウェブアプリはリーチに優れていることを示します。では、プログレッシブウェブアプリはどのような位置付けになるのでしょうか？

プログレッシブウェブアプリ（PWA）は、単一のコードベースで、拡張された機能、信頼性、およびインストール可能性を提供しながら、*場所やデバイスを問わず、あらゆるユーザー*に達することができるよう、最新のAPIで構築、拡張されています。

## アプリの3つの柱

プログレッシブウェブアプリは、高い機能を持ち、信頼性があり、インストールが可能となるように設計されたウェブアプリケーションです。これら3つの柱があるおかげで、プログレッシブウェブアプリは、まるでプラットフォーム固有のアプリケーションであるかのように感じられます。

### 高い機能を持つ

今日、ウェブはそれだけでも非常に有能です。たとえば、WebRTC、ジオロケーション、プッシュ通知を使用すれば、ハイパーローカルビデオチャットアプリを構築できます。そのアプリをインストール可能にし、WebGLとWebVRを使用することでそれらの会話を仮想化することができます。 Web Assemblyの導入により、開発者はC、C ++、Rustといった他のエコシステムを利用して、数十年分に相当する作業と機能をWebに導入することができます。たとえば、[Squoosh.app](https://squoosh.app/)は、それを活用して高度な画像圧縮を行います。

最近まで、そうした機能を独自のものとして主張できたのはプラットフォーム固有のアプリだけでした。一部の機能はまだWebの手の届かないところにありますが、新しいAPIと今後のAPIはその現状を一変させ、Webにおけるファイルシステムアクセスやメディアコントロール、アプリバッジ、クリップボードの完全サポートなどの用途を拡張しようとしています。これらの機能はすべて、Webの安全なユーザー中心のアクセス許可モデルで構築されているため、Webサイトにアクセスする際にユーザーがビクビクすることはありません。

最新のAPI、Web Assembly、および新しいAPIと今後のAPIの間で、Webアプリケーションはこれまで以上に機能が向上し、その勢いは止まりません。

### 信頼性がある

信頼性の高いプログレッシブウェブアプリは、ネットワークを問わず、高速で信頼性が高いと感じられます。

ユーザーにサイトを*使用*してもらうには、スピードが欠かせませ。実際、ページの読み込み時間が1秒から10秒になると、ユーザーがサイトを去ってしまう確率は[123％増加](https://www.thinkwithgoogle.com/marketing-resources/data-measurement/mobile-page-speed-new-industry-benchmarks/)します。パフォーマンスは、`onload`イベント以降も続きます。たとえば、ユーザーがボタンをクリックする場合など、サイトとのやり取りがちゃんと登録されているのかをユーザーに心配させてしまうようではいけません。スクロールやアニメーションはスムーズに感じられなくてはいけません。パフォーマンスは、ユーザーによるアプリケーションの認識から実際のパフォーマンスにいたる体験のすべてに影響します。

最後に、信頼できるアプリケーションは、ネットワーク接続の状態を問わず使用できる必要があります。ユーザーは、ネットワーク接続が遅くても、不安定でも、またオフラインの場合でも、アプリが起動することを期待します。ユーザーは、サーバーにリクエストを送信するのが困難であったとしても、メディアトラックやチケット、旅程など、前回操作したコンテンツが利用可能であることを期待します。リクエストを送信できない場合、ユーザーは、アプリが何の表示もせずにエラーになったり、クラッシュしたりするのではなく、問題が発生しているという表示が出ることを期待します。

ユーザーは、自分の操作にすかさず反応するアプリと信頼できるエクスペリエンスを好みます。

### インストール可能

インストールされたプログレッシブウェブアプリは、ブラウザタブではなくスタンドアロンウィンドウで実行されます。これらは、ユーザーのホーム画面、ドック、タスクバー、またはシェルフから起動できるほか、デバイス上で検索し、アプリスイッチャーを使って切り替えることができるため、まるでインストールされているデバイスに組み込まれてあるかのように感じます。

ウェブアプリがインストールされると、新しい機能が開きます。通常、ブラウザで実行するときに予約されているキーボードショートカットが利用可能になります。プログレッシブウェブアプリは、他のアプリケーションからのコンテンツを受け入れるように登録したり、さまざまな種類のファイルを処理するためのデフォルトのアプリケーションとして登録したりできます。

プログレッシブウェブアプリがタブではなくスタンドアロンアプリのウィンドウで稼働するようになると、ユーザーの考え方や操作方法が変わります。

## 両方の長所を活かす

本質的に、プログレッシブウェブアプリは単なるウェブアプリケーションです。プログレッシブエンハンスメントを使用すると、最新のブラウザで新しい機能が有効になります。 サービスワーカーとウェブアプリのマニフェストを使用すると、ウェブアプリケーションは信頼性が高まり、とインストール可能となります。新しい機能が利用できない場合でも、ユーザーは主なエクスペリエンスを楽しめます。

数字は嘘をつきません！プログレッシブウェブアプリをリリースした企業は、目覚ましい成果を上げています。たとえば、Twitterでは、セッションあたりのページ数が65％増、ツイートが75％増、バウンス率が20％減、そしてアプリのサイズは97％以上小さくなりました。 日経では、PWAに切り替えた後、オーガニックトラフィックが2.3倍に増え、サブスクリプションが58%増、毎日のアクティブユーザーが49％増となりました。Huluは、プラットフォーム固有のデスクトップエクスペリエンスをプログレッシブウェブアプリに置き換えた結果、再訪問数が27％増加しました。

プログレッシブウェブアプリは、ユーザーが喜ぶウェブ体験を実現するユニークな機会を提供します。プログレッシブウェブアプリでは、強化された機能と信頼性を届ける最新のウェブ機能が活用されるため、*誰でも、どこからでも、どのデバイスを使ってでも*たった一つのコードベースを使ってアプリをインストールできます。
