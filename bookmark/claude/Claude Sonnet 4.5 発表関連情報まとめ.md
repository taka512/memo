---
title: "Claude Sonnet 4.5 発表関連情報まとめ"
source: "https://zenn.dev/schroneko/articles/claude-sonnet-4-5"
author:
  - "[[Zenn]]"
published: 2025-09-30
created: 2025-10-14
description:
tags:
  - "clippings"
image: "https://res.cloudinary.com/zenn/image/upload/s--6nEaACtn--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Sonnet%25204.5%2520%25E7%2599%25BA%25E8%25A1%25A8%25E9%2596%25A2%25E9%2580%25A3%25E6%2583%2585%25E5%25A0%25B1%25E3%2581%25BE%25E3%2581%25A8%25E3%2582%2581%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2581%25AC%25E3%2581%2593%25E3%2581%25AC%25E3%2581%2593%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzE4ZDE4NWExYTYuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT"
---
377

89

## tl;dr

- Claude Sonnet 4.5 が発表されたよ
- コーディング性能はあらゆるモデルの中で一番高いよ
- ミスアライメントがすべてのモデルの中で最も低いよ
- Opus 4.1 と比べても全体的に性能が向上しているよ
- Claude Code が v 2.0.0 にアプデされたよ
- Claude Code にチェックポイント機能がついたよ（ESC x2 か `/rewind` ）
- Claude Code SDK が Claude Agent SDK にリネームされたよ
- Claude for Chrome がウェイトリスト登録者全員に解放されたよ
- Claude API にコンテキスト管理のふたつの機能が追加されたよ
- Claude Sonnet 4.5 に既に対応済みのサービスをまとめたよ

---

Claude Sonnet 4.5 の発表にともなって公開されたリソースが盛りだくさんで、迷子になりかけたのでまとめます。各リソースに対してそれぞれ解説をしていきます。

## Claude Sonnet 4.5 について

Anthropic の Claude Sonnet 4.5 が公開。世界中で最高性能のコーディングモデル。複雑なエージェントの構築やコンピュータ操作、リーズニング、数学タスクにおいて前のモデルから大幅に性能向上。30 時間を超える複雑な複数のステップにわたるタスクを実行可能。入力 $3 / 1M token、出力 $15 / 1M token とお値段は据え置き。

### Claude Sonnet 4.5 の性能

![](https://storage.googleapis.com/zenn-user-upload/61fce0feda1e-20250930.webp)

ソフトウェアエンジニアリングタスクのベンチマーク SWE-bench Verified において 77.2% と最高性能。実際の開発では、複雑で複数のステップにわたるタスクにおいて、30 時間以上かけてタスクを実行できることを確認。実世界のコンピュータ操作の能力を測るベンチマーク OSWorld においても 61.4% と最高性能。

下記のデモ動画では、Claude がブラウザを直接操作し、ウェブサイトを操作したりスプレッドシートへ入力したりする様子を確認できる。

<iframe src="https://www.youtube-nocookie.com/embed/oXfVkbb7MCg" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>

![](https://storage.googleapis.com/zenn-user-upload/9b91b06a2abc-20250930.webp)

他にも多くのベンチマークで性能向上を確認。

![](https://storage.googleapis.com/zenn-user-upload/af9437f24b83-20250930.webp)

![](https://storage.googleapis.com/zenn-user-upload/b0a2d753251b-20250930.webp)

![](https://storage.googleapis.com/zenn-user-upload/be162f9bc6d2-20250930.webp)

![](https://storage.googleapis.com/zenn-user-upload/81a195d25e2a-20250930.webp)

金融、法律、医療、科学技術分野の専門家による評価では Sonnet 4.5 は Opus 4.1 を含むモデルと比較して、専門分野に関する知識とリーズニング能力が格段に向上したとのこと。

アーリーアクセスのあった Cursor や GitHub、NETFLIX 他の企業においても高評価。

### ミスアライメントを軽減したモデル

![](https://storage.googleapis.com/zenn-user-upload/3f24d1f5a109-20250930.webp)

Claude Sonnet 4.5 は性能の高さだけではなく、ミスアライメントを最小にまで減らすことのできたモデル。安全性の訓練により、過度にユーザの指示に従ったり、虚偽の回答をするなどの行動パターンを低減。プロンプトインジェクションに対する防御性能を大幅に向上。

また、Claude Sonnet 4.5 は AI セーフティレベル 3（ASL-3）のもと公開。化学兵器・生物兵器・放射性物質・核兵器（CBRN）に関する入出力を検出するための「分類器」と呼ばれるフィルタを含む。

時折、分類器の誤検知が想定されるため、会話が中断された場合 Claude Sonnet 4 に切り替える機能を実装。誤検知は継続的に減少しており、さらに減らせるよう取り組んでいる。

### Claude Agent SDK

半年以上に渡って Claude Code を継続的に改善。メモリ管理や権限システム、サブエージェントについて取り組んできた。コーディングに限らず、様々なタスクにおいて役に立つツールとして、Claude Code SDK を Claude Agent SDK とリネームしてリブランディング。

### Bonus research preview

リサーチプレビュー版の「Imagine with Claude」を公開。Claude がリアルタイムでソフトウェアを作成。対話に応じてその場で Claude が作成していく様子を眺められる。Max プランの契約が必須で、遊べるのは 5 日間だけ。

### 補足

あらゆる用途において Claude Sonnet 4 の完全上位互換であり、Claude Sonnet 4.5 へのアップグレードを推奨。

## AI エージェント向けの適切なコンテキストエンジニアリング

プロンプトエンジニアリングとは、LLM に対する最適な結果を得るための指示文の書き方や構造化の仕方。一方で、コンテキストエンジニアリングとは、LLM の推論時に用いる最適な情報を（トークンセット）を選び、管理するための戦略であり、システムインストラクション、ツール、MCP、外部データ、メッセージ履歴など、プロンプト以外の関連するすべての情報が含まれる。絶えず変化する膨大な情報から、限られたコンテキストウィンドウに含めるべき内容を選別・構築する芸術と科学。

![](https://storage.googleapis.com/zenn-user-upload/01c38e91ffad-20250930.png)

人間と同じように LLM は情報が多くなればなるほど、そのコンテキストから情報を正確かつ効率的に引っ張ってくる能力が低下、これを「context rot」と呼ぶ。位置エンコーディングなどにより、ロングコンテキストの処理はできるようになるが、検索や推論においてコンテキストが短い時に比べて性能が下がることがある。したがって、適切なコンテキスト設計が重要。

優れたコンテキスト設計とは、求める結果が得られる可能性を最大化しつつ、可能な限り最小限の情報で構成されるトークンセットを見つけることを意味する。

具体的には、システムプロンプトを適切な抽象度で書き、ツールは機能が重複しないよう明確に与え、例示はかさ増すことなく厳選する。実行時は事前にコンテキストをすべて読み込むのではなく、ファイルパスやリンクなどの軽い識別子を保持しておき、必要に応じて動的に情報を取得する「ジャストインタイム」アプローチが適切。これは人間の認知プロセスに似ており、Claude Code でも採用されている。

コンテキストウィンドウを超える長時間のタスクには、要約して新たなコンテキストウィンドウで再開するコンテキスト圧縮、コンテキスト外にメモを保存する構造化メモ、専門エージェントが個別処理するサブエージェントアーキテクチャといった技術がある。

## Claude Agent SDK を使ってエージェントを構築

Claude Code の公開当初は Anthropic 社内の開発者の生産性向上が目的であった。ここ数ヶ月で単なるコーディングツールの枠を超え、Deep Research や動画制作、メモといったコーディング以外の用途にも利用が進む。汎用的に使えるということを名前に反映させるため、Claude Code SDK を Claude Agent SDK にリネーム。

Claude にコンピュータ操作の機能を与えることで、CSV ファイルの読み取りやウェブ検索、データの可視化など、さまざまな作業をこなす汎用エージェントに。Claude Agent SDK で作成できるエージェントの例として、財務管理エージェント、パーソナルアシスタントエージェント、カスタマーサポートエージェント、ディープリサーチエージェントなどが挙げられている。

![](https://storage.googleapis.com/zenn-user-upload/b8b673386223-20250930.png)

エージェントは「コンテキスト収集→アクション実行→検証→繰り返し」のエージェントループで動作。コンテキスト収集にはエージェンティック検索、セマンティック検索、サブエージェント、コンパクション機能を、アクション実行には、ツール、bash、コード生成、MCP を、検証にはルール定義、ビジュアルフィードバック、LLM による評価を用いる。

エージェントループを繰り返した後に、各タスクが適切な状態かどうかをテスト。出力結果、特に失敗事例を分析。特に必要なツールが与えられているかを確認。機能追加などにより、評価にブレが生じる場合は実際のお客さんの利用パターンに基いたテストセットを用意。

## Claude Agent SDK へのマイグレーション

Claude Code SDK が Claude Agent SDK にリネームされたことによるマイグレーションガイド。

![](https://storage.googleapis.com/zenn-user-upload/27bc08335e1e-20250930.png)

一度アンインストールしてから Claude Agent SDK をインストール。

```
npm uninstall @anthropic-ai/claude-code
```

```
npm install @anthropic-ai/claude-agent-sdk
```

```
pip uninstall claude-code-sdk
```

```
pip install claude-agent-sdk
```

ドキュメントのコードのすべてを貼り付けると長くなるのでしませんが、TypeScript にせよ、Python にせよ、インポート箇所の置き換えが必要。また、SDK 経由で CLAUDE.md や settings.json、スラッシュコマンドなどを読み込まないように。したがって、使いたい時には下記のように指定する必要がある。

```
const result = query({
  prompt: "Hello",
  options: {
    settingSources: ["user", "project", "local"]
  }
});
```

## Claude Code をより自律的に

Claude Code にいくつかのアップグレードが加えられ、VS Code 拡張機能、ターミナルインターフェース 2.0、チェックポイント機能が追加。デフォルトモデルに Claude Sonnet 4.5 を採用。

ネイティブの VS Code 拡張機能により、IDE にてサイドバーから直接 Claude Code を使うことができ、Claude の編集をリアルタイムで確認できる。

<iframe src="https://www.youtube-nocookie.com/embed/IpFG_K-1xog" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>

ターミナルインターフェースにおいて、ステータス表示を改善、プロンプトの履歴機能を `Ctrl+r` で検索可能に。プロンプトの再利用や編集が簡単に。

![](https://storage.googleapis.com/zenn-user-upload/148f2e447b40-20250930.webp)

チェックポイント機能が追加。変更前のタイミングでコードを自動保存、Esc キーを 2 回、あるいは `/rewind` コマンドで前のバージョン戻すことができる。復元の際には、コード、会話、またはその両方を選ぶことができる。

ただし、Claude による編集時にのみ適用されるため、ユーザによる編集や bash コマンドの実行時には適用されない点に注意。バージョン管理システムとの併用を推奨。

![](https://storage.googleapis.com/zenn-user-upload/adee0b0f53c3-20250930.png)

## Claude Developer Platform におけるコンテキスト管理

Claude API にて、コンテキスト管理を行なうための「コンテキスト編集機能」と「メモリツール」のふたつの機能を追加。開発者はコンテキストの上限に悩むことなく、長時間にわたるタスクを処理するエージェントを構築することができる。

### 実業務ではコンテキストウインドウのような制約は存在しない

コンテキスト編集機能は、トークンの上限に近付くとコンテキストウインドウにあるツール呼び出し結果のうち、古いものを自動的に削除するもの。

![](https://storage.googleapis.com/zenn-user-upload/09a4d105bb19-20250930.png)

メモリツールは、コンテキストウィンドウの外にある情報を保存、参照することができるもの。専用のメモリディレクトリにおいて、ファイルの作成や読み込み、更新、削除が可能。

Claude がコンテキスト編集機能やメモリツールを使ってカタンをプレイする様子。ナレッジベースを構築、ゲームを横断してメモリを保持、古くなった情報を削除する。

<iframe src="https://www.youtube-nocookie.com/embed/BER3EhUIyz0" allow="accelerometer; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen=""></iframe>

メモリツールはクライアント側で動作。ストレージはバックエンドで管理、データの保存先や永続化は制御可能。メモリは後続のエージェントのセッションで使うこともできる。

長時間稼働するエージェントの活用事例として挙げられているものは、コーディング作業、研究、データ処理。

コンテキスト編集機能とメモリツールの組み合わせで、ベースラインと比べて複雑な複数のステップのタスクの性能が 39% 向上。コンテキスト編集機能単体でも 29% の性能向上。通常であればコンテキストが溢れて失敗していたタスクもコンテキスト編集機能単で完遂可能に、トークン消費量も 84% 削減。

## Claude Sonnet 4.5 のメモリ＆コンテキスト管理

Claude Sonnet 4.5 のメモリツールとコンテキスト管理機能についての Jupyter Notebook によるクックブック。

メモリツール（ノートブック内の memory\_20250818）を呼び出し、学んだパターンをファイルベースで記録、クライアント側の実装により制御。

コンテキスト編集機能（clear\_tool\_uses\_20250919）により、ツール結果のうち古いものを自動削除、メモリを保持したまま会話を管理。 `"edits": [{"type": "clear_tool_uses_20250919",...` のように与える。メモリは /memories ディレクトリに格納。

## Claude for Chrome 拡張機能

Claude がブラウザ操作をしてくれる Google Chrome の拡張機能。Claude Max プランが必須。$100 か $200 は問わない。以前より、ウェイトリスト登録をされていた方は使えるようになっているはず。もしウェイトリスト登録がまだの方は下記の案内とウェイトリスト登録ページを参照。

Claude for Chrome のセットアップや使い勝手について以前まとめているので、ご興味のある方がいらしたらご覧ください。

## Claude Code for VS Code （VS Code 拡張機能）

バージョン 2.0.0 にアップデート。ネイティブ統合。

## Claude の利用状況が設定から見られるように

Claude のアプリと Claude Code の利用状況をリアルタイムで確認できるように。下記ページより。

こんな感じ。カレントセッションとウィークリーリミットを見ることができる。Opus 単体も。

![](https://storage.googleapis.com/zenn-user-upload/a1896597078d-20250930.png)

Claude Code にも `/usage` コマンドが追加されたのでそちらはこんな感じ。

![](https://storage.googleapis.com/zenn-user-upload/c0b85691cd0f-20250930.png)

## Claude Code 2.0.0 のアップデート内容

Claude Code のバージョンが 1.0.126 から 2.0.0 に。変更内容は下記の通り。

- VS Code のネイティブ拡張機能
- アプリケーションの全面的な機能改善
- 会話を遡って変更を取り消し `/rewind` コマンドを追加
- 利用状況を確認する `/usage` コマンドを追加
- 思考モードの切り替えをタブキーで（セッション内で保持）
- Ctrl-R で履歴を検索
- 未実装だった Claude コンフィグコマンドを追加？よくわからんけど config は増えていることを確認
- フック機能の改善（ `'tool_use' ids without 'tool_result' blocks` エラー）
- Claude Code SDK の Claude Agent SDK への名称変更
- `--agents` フラグを用いてサブエージェントを動的に追加

## 各社の Claude Sonnet 4.5 対応状況

Cursor、Windsurf、OpenRouter、Cline、Vercel、Bedrock、Vertex AI、Devin、Genspark、Databricks、GitHub Copilot、Perplexity AI、GitLab Duo にて利用可能。

---

以上となります！お読みいただきありがとうございます。

モデルだけでなく、ツールやプロダクト、ボーナスプレビューまで盛りだくさん！10 月 6 日の OpenAI DevDay になんとしても間に合わせたかったという気概が伝わってきます...！私ごとですが、そんな来たる OpenAI DevDay に運よく現地参加することがかないまして、半月ほどサンフランシスコの AI スタートアップに訪問予定です。もし気になる方がいらしたら、X のサブスクにて情報を公開すると思うので、入ってくださると泣いて喜びます！

ちなみに、今週は Gemini 関連の発表もあるようですので、とても寝る時間がありませんね（）

## おまけ 1

マスコットキャラクターの名前は「Claw'd」。Claude Artifacts で SVG で作成されたキャラクターです。おそらく読みは同じ「クロード」かな？

## おまけ 2

システムプロンプトをリークさせてみました。ご参考まで。

[GitHubで編集を提案](https://github.com/schroneko/zenn/blob/main/articles/claude-sonnet-4-5.md)

377

89

この記事に贈られたバッジ

![Thank you](https://static.zenn.studio/images/badges/paid/badge-frame-10.svg) ![Thank you](https://static.zenn.studio/images/badges/paid/badge-frame-10.svg) ![Thank you](https://static.zenn.studio/images/badges/paid/badge-frame-5.svg) ![Thank you](https://static.zenn.studio/images/badges/paid/badge-frame-5.svg) ![Thank you](https://static.zenn.studio/images/badges/paid/badge-frame-3.svg)