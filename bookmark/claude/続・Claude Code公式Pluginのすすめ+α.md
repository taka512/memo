---
title: "続・Claude Code公式Pluginのすすめ+α"
source: "https://zenn.dev/modokkin/articles/zenn-2026-01-06-claude-code-plugins-update"
author:
  - "[[Zenn]]"
published: 2026-01-06
created: 2026-01-20
description:
tags:
  - "clippings"
image: "https://res.cloudinary.com/zenn/image/upload/s--hpvcreCS--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:%25E7%25B6%259A%25E3%2583%25BBClaude%2520Code%25E5%2585%25AC%25E5%25BC%258FPlugin%25E3%2581%25AE%25E3%2581%2599%25E3%2581%2599%25E3%2582%2581%252B%25CE%25B1%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2581%25AF%25E3%2581%25B6%25E3%2581%25A1%25E3%2582%2593%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2Q4ZDhlMmQyZTAuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT"
---
97

44[tech](https://zenn.dev/tech-or-idea)

こんにちは、株式会社 tacoms で SRE をやっている はぶちん ([@modokkin](https://x.com/modokkin)) です。  
2025年が終わり2026年になりましたね。今年もどうぞよろしくお願いします。

さて、今回は前回の記事の続編として、Claude Codeの最近のアップデートで追加された新機能や便利になったポイントを紹介します。

## 前回記事のおさらい

前回は公式Pluginの概要とインストール方法、実際に使ってみた感想を紹介しました。

記事の中で、公式マーケットプレイスを手動で追加する手順を紹介しました。しかし2025年12月中旬のアップデートでこの手順が不要になりました。その後も継続的にアップデートがあるので、2026年1月時点の最新情報を紹介します。

## 最新のアップデート内容（2026年1月時点）

### 1\. Marketplace標準搭載

以前は手動で `claude-plugins-official` マーケットプレイスを追加する必要がありましたが、最新版ではデフォルトで登録されるようになりました。

```bash
> /plugin
  Discover   Installed   Marketplaces   Errors  (tab to cycle)

Manage marketplaces

❯ + Add Marketplace

  ● ✻ claude-plugins-official ✻
    anthropics/claude-plugins-official
    29 available • Updated 12/17/2025
```

これにより、初回セットアップの手間がなくなり、Claude Codeのインストール直後から公式Pluginをすぐに使い始められます。

### 2\. 自動アップデート機能

公式Pluginはデフォルトで自動アップデートが有効になりました。これにより、常に最新の機能とバグフィックスが適用されます。

現状は公式Pluginに限定されており、その他のマーケットプレイスから取得したPluginについては手動設定が必要です。セキュリティとバージョン管理のバランスを考慮した仕様なのでしょうね。

### 3\. 新規MCPプラグインの大量追加

前回の記事執筆時（12月初旬）から、多くのMCPプラグインが追加されました。以下は追加されたPluginの一覧です。

| Plugin名 | カテゴリ | 説明 |
| --- | --- | --- |
| sentry | monitoring | エラーモニタリングサービスとの連携 |
| slack | productivity | Slack連携 |
| vercel | deployment | Vercelデプロイ |
| firebase | database | Firebase連携 |
| figma | design | Figmaデザイン連携 |
| asana | productivity | タスク管理 |
| linear | productivity | Issue管理 |
| Notion | productivity | Notion連携 |
| gitlab | productivity | GitLab連携 |
| playwright | testing | E2Eテスト |
| github | productivity | GitHub連携 |
| supabase | database | Supabase連携 |
| atlassian | productivity | Jira/Confluence連携 |
| laravel-boost | development | Laravel開発 |
| greptile | development | AIコード検索 |
| serena | development | セマンティックコード分析 |
| context7 | development \[Community Managed\] | ライブラリドキュメント取得 |

開発ツールだけでなく、デザインツール（Figma）やプロジェクト管理ツール（Asana、Linear、Jira）との連携も可能になりました。以前からClaude Desktopで対応していたものがClaude Codeでも簡単に設定できるようになった感じでしょうか。

MCPは、有効化すると起動するだけで結構コンテキストを消費してしまうので、作業によって必要な時だけ有効化することをおすすめします。たとえば基本は無効化しておいて、Worktreeで作業するときに必要なものだけを手動で有効化するとかでも良いと思います。

...と、書こうとしていたら公式が非常に便利な機能をリリースしてくれました。

MCPツールを動的に読み込む機能が実験的に追加されています。

Oikonさん、いつも参考にさせていただいてます。ありがとうございます。

公式ドキュメントで紹介されて気になっていたのですが、ついにClaude Codeにも組み込まれたようです。

Claude Codeでこの機能を試すには、環境変数または `settings.json` で以下を指定します。

```json
{
  "env": {
    "ENABLE_TOOL_SEARCH": "true",
    "ENABLE_EXPERIMENTAL_MCP_CLI": "false"
  }
}
```

この機能を有効にすると、MCPツールの定義が初期ロード時にはコンテキストへ読み込まれなくなります。必要な時だけ動的に検索して使用できるため、コンテキストの節約効果が大きいです。

なお、Claude CodeでのこのオプションはCHANGELOGには記載されておらず、GitHub上ではFeature Requestとして議論されています。

実験的機能なので今後変更される可能性はありますが、気になる方はお試しください。

実際に設定してみたところ、 `/context` コマンドで確認すると「MCP tools · /mcp (loaded on-demand)」と表示され、正しく有効になっていることが確認できました。

MCPはコンテキストを食い過ぎる問題もあり、SNSなどでは懐疑的な意見も見受けられます。しかし今回の公式Marketplace対応や以下のような取り組みを見ると、まだまだ普及させようという意気込みを感じますね。

### 4\. context7のプラグイン版

前回の記事で紹介したcontext7にプラグイン版が追加されました。

MCP直接インストール版との違いとして、Plugin版では以下のツールが削減されています。

- `mcp__ide__getDiagnostics` （611 tokens）
- `mcp__ide__executeCode` （682 tokens）

約1,300トークン分のコンテキストを節約できます。Claude Codeにとっては不要なコンテキストが削減されているのだろうと思われます。

### 5\. 公式機能拡張Plugin

MCPサーバー連携とは別に、Claude Code自体の機能を拡張する公式Pluginも13個用意されています。各Pluginの詳細は前回の記事で紹介していますので、そちらをご覧ください。

中でも私がその後もよく使用しているPluginを紹介します。

| Plugin/コマンド | 用途 |
| --- | --- |
| `/feature-dev:feature-dev` | 新機能開発を7フェーズで体系的に進める |
| `/commit-commands:commit-push-pr` | ブランチ作成からPR作成まで一括実行 |
| `/pr-review-toolkit:review-pr` | 6つの専門エージェントでPRを多角的にレビュー |
| `security-guidance` | ファイル編集時にセキュリティリスクを自動検出 |
| `explanatory-output-style` | 実装選択の理由を説明（理解が浅いシチュエーションでおすすめ） |

### 6\. LSPツールの追加

最新版（v2.0.74）では、LSP（Language Server Protocol）ツールが追加されました。

- Go-to-definition（定義へジャンプ）
- Find references（参照検索）
- Hover documentation（ホバードキュメント）

これにより、Claude Codeがコードベースをより正確に理解できるようになっています。

### 7\. その他の改善

- Ctrl+Tでシンタックスハイライトのオン/オフを切り替え可能に
- `/plugin discover` 画面での検索機能が追加
- Kitty、Alacritty、Zed、Warpターミナルのセットアップサポート追加

## プロジェクトごとのプラグイン管理

今回のアップデートで、プロジェクトごとにPluginを管理できる機能が明確化されました。この機能により、チームメンバー全員で同じPlugin設定を共有したり、個人用のPluginをgit管理から除外したりできます。

### 設定の優先順位

Claude Codeの設定には3つのスコープがあり、優先順位は以下の通りです。

```
ローカル > プロジェクト > ユーザー
```

### スコープ別のインストール方法

Pluginをインストールする際に、 `--scope` オプションでスコープを指定できます。

```bash
# ユーザースコープ（全プロジェクトで有効、デフォルト）
/plugin install formatter@your-org

# プロジェクトスコープ（プロジェクト内で共有）
/plugin install formatter@your-org --scope project

# ローカルスコープ（個人用、git無視）
/plugin install formatter@your-org --scope local
```

## 実践的な活用例

### チーム開発での活用

プロジェクトスコープを使えば、チーム全体で統一したPlugin環境を構築できます。

```bash
# チーム全員で使うレビューツール
/plugin install pr-review-toolkit@claude-plugins-official --scope project

# チーム全員で使うコミットツール
/plugin install commit-commands@claude-plugins-official --scope project

# プロジェクトの設定ファイルをコミット
git add .claude/settings.json
git commit -m "feat: Claude Codeのプラグイン設定を追加"
git push
```

これにより、新しいメンバーがプロジェクトをクローンした際、自動的に同じPlugin環境が構築されます。

### 個人実験での活用

一方で、個人的に試してみたいPluginはローカルスコープでインストールすることで、チームに影響を与えずに実験できます。

```bash
# 個人で試してみたい実験的なPlugin
/plugin install experimental-feature@test --scope local
```

このPluginの設定は `.claude/settings.local.json` に保存され、gitには含まれません。

## まとめ

Claude Codeの公式Pluginは、2025年12月から2026年1月にかけてのアップデートでさらに使いやすくなりました。

特に以下の点が改善されました。

- Marketplace標準搭載で、初回セットアップが簡単に
- 自動アップデート機能で常に最新の機能が利用可能に
- 多数のMCPサーバー連携Pluginが追加され、外部サービスとの連携が簡単に
- 13個の機能拡張Pluginにより、コードレビューやセキュリティ監視などを強化
- LSPツール追加でコードベースの理解がより正確に
- プロジェクトごとのPlugin管理により、チーム開発と個人実験を両立

前回の記事を読んで「手動でマーケットプレイスを追加するのは面倒だな」と思った方も、今ならすぐに始められます。特にチーム開発でClaude Codeを活用する場合、 `pr-review-toolkit` や `feature-dev` といった機能拡張Pluginはめちゃくちゃ便利だと思います。

まだPluginを試していない方は、ぜひこの機会に公式Pluginを導入してみてください。

最後までお読みいただきありがとうございます。どなたかの参考になれば幸いです。

## 参考リンク

- [Claude Code公式リポジトリ](https://github.com/anthropics/claude-code)
- [公式Plugins](https://github.com/anthropics/claude-code/tree/main/plugins)

97

44