---
title: "テスト自動化フレームワーク「Playwright」にAIエージェント機能。自動的にテスト計画とテストコードの生成、テストコードのデバッグなど"
source: "https://www.publickey1.jp/blog/25/playwrightai.html"
author:
published:
created: 2025-10-14
description: "マイクロソフトが主導して開発しているオープンソースのテスト自動化フレームワーク「Playwright」の新バージョン「Playwright 1.56」がリリースされました。 Playwrightはデスクトップ向けのWebアプリケーションやモ..."
tags:
  - "clippings"
image: "https://www.publickey1.jp/2025/maxresdefault.jpg"
---
2025年10月14日

  

マイクロソフトが主導して開発しているオープンソースのテスト自動化フレームワーク「Playwright」の新バージョン「 [Playwright 1.56](https://playwright.dev/docs/release-notes#version-156) 」がリリースされました。

Playwrightはデスクトップ向けのWebアプリケーションやモバイル向けのWebアプリケーションのテスト自動化が可能で、Chromium、WebKit、Firefoxなどの主要なモダンブラウザのエンジンのヘッドレス、ヘッドありのいずれの環境でも実行可能です。

OSはWindows、macOS、Linuxをサポートします。

テストはJavaScript、TypeScript、Java、Python、C＃などで記述でき、ローカル環境での実行だけでなく、CI（継続的統合）環境での実行などにも対応します。

Playwright 1.56では、新機能として生成AIによる以下の3つのエージェントが追加されました。

*Planner*  
対象となるWebアプリケーションのテスト計画をMarkdown形式のドキュメントとして生成

*Generator*  
テスト計画のドキュメントをPlaywrightで実行可能なテストコードに変換

*Healer*  
テストが失敗した場合など、テストコードのバグを修正する

## テスト計画やテストコードなど自動生成

PlaywrightのAIエージェント機能はVisual Studio CodeのGitHub CopilotもしくはClaude Code、OpenCodeに対応しています。

下記はVisual Studio Codeによるデモ動画の画面です。

![fig](https://www.publickey1.jp/2025/playwright156-01.png)

例えば、ユーザーが視聴した映画を管理する機能を備えたWebサイトに対して、ログインなどのテスト環境を整えた上で、Plannerエージェントに「映画のリスト管理機能のテストを生成せよ」と指示すると、テスト環境の初期設定に従って実際にWebサイトにログイン、機能を探索、分析を開始します。

オプションとしてProduct Requirements Document（PRD）をコンテキストを明示するために追加可能です。

![fig](https://www.publickey1.jp/2025/playwright156-02.png)

そしてヒューマンリーダブルかつ、テストコードを生成するのにも十分な内容を備えたテスト計画のドキュメントがMarkdown形式で生成されます。

![fig](https://www.publickey1.jp/2025/playwright156-03.png)

テスト計画の内容に問題がなければ、Generatorエージェントに渡すことでPlaywrightが実行可能なテストコードが生成されます。

テストが失敗する場合には、Healerエージェントに対して修正を指示することでテストコードの問題を見つけ、修正してくれます。

![fig](https://www.publickey1.jp/2025/playwright156-04.png)

#### あわせて読みたい

- [ガートナー、AIにおけるハイプサイクル2025を発表。AIエージェントやマルチモーダルAIは過剰期待、AIネイティブソフトウェアエンジニアリングやAGIは黎明期など](https://www.publickey1.jp/blog/25/ai2025aiaiaiagi.html)
- [生成AIに疑似コードで指示すると自然言語よりも効率的にプログラムが生成できるというアイデアから生まれた、生成AI用の疑似言語「SudoLang」](https://www.publickey1.jp/blog/24/aiaisudolang.html)
- [マイクロソフト、AIエージェントがセキュリティ対応を自動化。Security Copilotエージェント群を発表](https://www.publickey1.jp/blog/25/aisecurity_copilot.html)
- [生成AIが自律的にテストを生成、実行しバグや脆弱性を発見してくれる「Spark」、Code Intelligenceが正式リリース](https://www.publickey1.jp/blog/25/aisparkcode_intelligence.html)

[![fbシェア](https://www.publickey1.jp/2024/fbshare_btn.png)](http://www.facebook.com/share.php?u=https%3A%2F%2Fwww.publickey1.jp%2Fblog%2F25%2Fplaywrightai.html)

[![Xポスト](https://www.publickey1.jp/2024/xpost_btn.png)](https://twitter.com/intent/tweet?original_referer=https%3A%2F%2Fwww.publickey1.jp%2F&text=%E3%83%86%E3%82%B9%E3%83%88%E8%87%AA%E5%8B%95%E5%8C%96%E3%83%95%E3%83%AC%E3%83%BC%E3%83%A0%E3%83%AF%E3%83%BC%E3%82%AF%E3%80%8CPlaywright%E3%80%8D%E3%81%ABAI%E3%82%A8%E3%83%BC%E3%82%B8%E3%82%A7%E3%83%B3%E3%83%88%E6%A9%9F%E8%83%BD%E3%80%82%E8%87%AA%E5%8B%95%E7%9A%84%E3%81%AB%E3%83%86%E3%82%B9%E3%83%88%E8%A8%88%E7%94%BB%E3%81%A8%E3%83%86%E3%82%B9%E3%83%88%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AE%E7%94%9F%E6%88%90%E3%80%81%E3%83%86%E3%82%B9%E3%83%88%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AE%E3%83%87%E3%83%90%E3%83%83%E3%82%B0%E3%81%AA%E3%81%A9%20%EF%BC%8D%20Publickey&url=https%3A%2F%2Fwww.publickey1.jp%2Fblog%2F25%2Fplaywrightai.html)

[![Feedly](https://www.publickey1.jp/2024/feedly_btn.png)](https://feedly.com/i/subscription/feed%2Fhttps%3A%2F%2Fwww.publickey1.jp%2Fatom.xml)

  

*≫次の記事*  
[Python 3.14登場、フリースレッド版正式サポート。実験的JITコンパイラも公式バイナリで利用可能に](https://www.publickey1.jp/blog/25/python_314jit.html)  
  
*≪前の記事*  
[Google、「Gemini Enterprise」発表。OfficeやSalesforce、SAP、Jiraなど業務アプリとの連携、コーディング支援、エージェント開発まで包括的に提供](https://www.publickey1.jp/blog/25/googlegemini_enterpriseofficesalesforcesapjira.html)

  
  

#### タグクラウド

[クラウド](https://www.publickey1.jp/cloud/)  
[AWS](https://www.publickey1.jp/cloud/aws/) / [Azure](https://www.publickey1.jp/cloud/microsoft-azure/) / [Google Cloud](https://www.publickey1.jp/cloud/google-cloud/)  
[クラウドネイティブ](https://www.publickey1.jp/cloud/cloud-native/) / [サーバレス](https://www.publickey1.jp/cloud/serverless/)  
[クラウドのシェア](https://www.publickey1.jp/cloud/cloud-share/) / [クラウドの障害](https://www.publickey1.jp/cloud/cloud-failure/)  

[コンテナ型仮想化](https://www.publickey1.jp/container-vm/)

[プログラミング言語](https://www.publickey1.jp/programming-lang/)  
[JavaScript](https://www.publickey1.jp/programming-lang/javascript/) / [Java](https://www.publickey1.jp/programming-lang/java/) / [.NET](https://www.publickey1.jp/programming-lang/net/)  
[WebAssembly](https://www.publickey1.jp/programming-lang/webassembly/) / [Web標準](https://www.publickey1.jp/programming-lang/web-standards/)  
[開発ツール](https://www.publickey1.jp/devtools/) / [テスト・品質](https://www.publickey1.jp/devtools/software-test/)

[アジャイル開発](https://www.publickey1.jp/devops/agile/) / [スクラム](https://www.publickey1.jp/devops/scrum/) / [DevOps](https://www.publickey1.jp/devops/)

[データベース](https://www.publickey1.jp/database/) / [機械学習・AI](https://www.publickey1.jp/database/machine-learning-ai)  
[RDB](https://www.publickey1.jp/database/rdb/) / [NoSQL](https://www.publickey1.jp/database/nosql/)  

[ネットワーク](https://www.publickey1.jp/network/) / [セキュリティ](https://www.publickey1.jp/network/security)  
[HTTP](https://www.publickey1.jp/network/http/) / [QUIC](https://www.publickey1.jp/network/quic/)

[OS](https://www.publickey1.jp/os) / [Windows](https://www.publickey1.jp/os/windows) / [Linux](https://www.publickey1.jp/os/linux) / [仮想化](https://www.publickey1.jp/os/vm)  
[サーバ](https://www.publickey1.jp/hardware/server/) / [ストレージ](https://www.publickey1.jp/hardware/storage/) / [ハードウェア](https://www.publickey1.jp/hardware/)

[ITエンジニアの給与・年収](https://www.publickey1.jp/trends/payment/) / [働き方](https://www.publickey1.jp/trends/workstyle/)

[殿堂入り](https://www.publickey1.jp/after-words/recommend/) / [おもしろ](https://www.publickey1.jp/after-words/funny) / [編集後記](https://www.publickey1.jp/after-words/)

[全てのタグを見る](https://www.publickey1.jp/tags.html)

#### Blogger in Chief

![photo of jniino](https://www.publickey1.jp/images/profile.jpg)

Junichi Niino（jniino）  
IT系の雑誌編集者、オンラインメディア発行人を経て独立。2009年にPublickeyを開始しました。  
（ [詳しいプロフィール](https://www.publickey1.jp/about-us.html) ）

Publickeyの新着情報をチェックしませんか？  
Twitterで ： [@Publickey](https://twitter.com/publickey/)  
Facebookで ： [Publickeyのページ](https://www.facebook.com/publickey/)  
RSSリーダーで ： [Feed](https://www.publickey1.jp/atom.xml)  

#### 最新記事10本

- [Python 3.14登場、フリースレッド版正式サポート。実験的JITコンパイラも公式バイナリで利用可能に](https://www.publickey1.jp/blog/25/python_314jit.html)
- [テスト自動化フレームワーク「Playwright」にAIエージェント機能。自動的にテスト計画とテストコードの生成、テストコードのデバッグなど](https://www.publickey1.jp/blog/25/playwrightai.html)
- [Google、「Gemini Enterprise」発表。OfficeやSalesforce、SAP、Jiraなど業務アプリとの連携、コーディング支援、エージェント開発まで包括的に提供](https://www.publickey1.jp/blog/25/googlegemini_enterpriseofficesalesforcesapjira.html)
- [「Diaブラウザ」正式版に、誰でもダウンロード可能。AI時代の業務のためのブラウザを目指す](https://www.publickey1.jp/blog/25/diaai.html)
- [「React Foundation」をメタ、マイクロソフト、Vercelらが設立へ。ReactやReact Nativeの中立的な開発主体として](https://www.publickey1.jp/blog/25/react_foundationvercelreactreact_native.html)
- [「State of JavaScript 2025」の投票がスタート、仕事や趣味でJavaScriptを使っている人は誰でも回答可能](https://www.publickey1.jp/blog/25/state_of_javascript_2025javascript.html)
- [生成AIはネットワーク構築運用をどれだけ楽にするか？ Interop Tokyoでの実験が明かした現実と可能性。Cloud Operator Days Tokyo 2025基調講演ほか［PR］](https://www.publickey1.jp/blog/25/ai_interop_tokyocloud_operator_days_tokyo_2025pr.html)
- [次期MCP（Model Context Protocol）では非同期操作、ステートレス、公式のプロトコル拡張などサポート](https://www.publickey1.jp/blog/25/mcpmodel_context_protocol.html)
- [マイクロソフト、MCPやA2Aプロトコルに対応したAIエージェント開発を容易にする「Microsoft Agent Framewok」プレビュー公開](https://www.publickey1.jp/blog/25/mcpa2aaimicrosoft_agent_framewok.html)
- [ガートナー「先進テクノロジーのハイプサイクル 2025年版」発表。人に代わるマシン・カスタマー、プログラマブル・マネーなど注目](https://www.publickey1.jp/blog/25/_2025.html)