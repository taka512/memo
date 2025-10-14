---
title: "AWS Knowledge MCP Server が GA されたので試しに設定してみたメモ✍"
source: "https://sadayoshi-tada.hatenablog.com/entry/2025/10/05/145509"
author:
  - "[[taddy (id:sadayoshi_tada)]]"
published: 2025-10-05
created: 2025-10-14
description: "タダです. AWS Knowledge MCP Server というのが GA されました.この MCP サーバーではドキュメント,ブログ記事,新機能の発表,Well-Architectedのベストプラクティスなどのデータにアクセスできるようなので普段の開発で AWS に触れる時に最新の情報を入手するのに役立ちそうなので設定してみます. aws.amazon.com awslabs.github.io AWS Knowledge MCP Server 概要 AWS Knowledge MCP Server の設定 Claude Desktop での設定 Claude Code での設定 試しに…"
tags:
  - "clippings"
image: "https://ogimage.blog.st-hatena.com/6435988827676416561/6802888565274811407/1760176724"
---
この記事は約6分で読めます。

タダです.

[AWS](https://d.hatena.ne.jp/keyword/AWS) Knowledge [MCP](https://d.hatena.ne.jp/keyword/MCP) Server というのが GA されました.この [MCP](https://d.hatena.ne.jp/keyword/MCP) サーバーではドキュメント,ブログ記事,新機能の発表,Well-Architectedのベストプ [ラク](https://d.hatena.ne.jp/keyword/%A5%E9%A5%AF) ティスなどのデータにアクセスできるようなので普段の開発で [AWS](https://d.hatena.ne.jp/keyword/AWS) に触れる時に最新の情報を入手するのに役立ちそうなので設定してみます.

[aws.amazon.com](https://aws.amazon.com/jp/about-aws/whats-new/2025/10/aws-knowledge-mcp-server-generally-available/)

[awslabs.github.io](https://awslabs.github.io/mcp/servers/aws-knowledge-mcp-server)

## AWS Knowledge MCP Server 概要

[AWS](https://d.hatena.ne.jp/keyword/AWS) Knowledge [MCP](https://d.hatena.ne.jp/keyword/MCP) Server はGA されましたが,主な機能として以下があります.

- [AWS](https://d.hatena.ne.jp/keyword/AWS) ドキュメント/ブログ/What's New/Well-Architectedベストプ [ラク](https://d.hatena.ne.jp/keyword/%A5%E9%A5%AF) ティスへのアクセス
- リージョンごとの [AWS](https://d.hatena.ne.jp/keyword/AWS) サービス利用可能状況の確認機能
- 無料かつ認証なしで利用可能(レートリミットあり)
- [AWS](https://d.hatena.ne.jp/keyword/AWS) アカウント不要

[MCP](https://d.hatena.ne.jp/keyword/MCP) サーバーで利用可能なツールは次の通りです.

- search\_documentation: [AWS](https://d.hatena.ne.jp/keyword/AWS) 全体のドキュメント検索
- read\_documentation: ドキュメントの [Markdown](https://d.hatena.ne.jp/keyword/Markdown) 変換
- recommend: 関連コンテンツの推奨
- get\_regional\_availability(Experimental): リージョン別サービス利用可否確認
- list\_regions(Experimental): 全 [AWS](https://d.hatena.ne.jp/keyword/AWS) リージョン一覧取得

## AWS Knowledge MCP Server の設定

この記事では普段使用している Cluade Desktop と Claude Code で試してみます.

### Claude Desktop での設定

以下のように設定して動作しました.

```json
{
  "mcpServers": {
        "aws-knowledge-mcp-server": {
            "command": "uvx",
            "args": [
                "fastmcp",
                "run",
                "https://knowledge-mcp.global.api.aws"
            ]
        }
    }
}
```

### Claude Code での設定

以下のように設定して動作しました.

```sh
claude mcp add \
    awslabs.aws-knowledge-mcp-server \
    -- uvx fastmcp run https://knowledge-mcp.global.api.aws
```

## 試しに最近のリリースについて質問してみる

[AWS](https://d.hatena.ne.jp/keyword/AWS) Knowledge [MCP](https://d.hatena.ne.jp/keyword/MCP) Server GA と同じ日に [AWS API MCP Server v1.0.0](https://aws.amazon.com/jp/about-aws/whats-new/2025/10/aws-api-mcp-server-v1-0-0-release/) がリリースされています.このことについて質問してみます.すると参照記事もあっており良さそうです.

```markdown
Q. AWS API MCP Server v1.0.0のリリース概要を教えて下さい

A. AWS API MCP Server v1.0.0のリリース概要をお伝えします。

## リリース概要

2025年10月、AWSはAWS API Model Context Protocol (MCP) Server v1.0.0を発表しました。このリリースにより、基盤モデル（FM）が自然言語を通じて任意のAWS APIとやり取りできるようになり、構文的に正しいCLIコマンドの作成と実行が可能になりました。

## 主な機能強化

### パフォーマンスと依存関係の改善
v1.0.0では、suggest_aws_commandツールをローカルインストールに依存するのではなくリモートサービスに変換することで、起動時間を短縮し、いくつかの依存関係を削除しました。

### セキュリティの強化
- 改善されたセキュアファイルシステムコントロール
- より良い入力検証機能
- 変更を伴うアクションに対して人間の監視と同意を要求するオプション設定

### 可観測性の向上
AWS CloudWatchエージェントを使用している顧客は、改善された可観測性のためにAPI MCP Serverからログを収集できるようになりました。

### 通信とホスティングオプションの拡張
より多くのホスティングと構成オプションをサポートするため、AWS API MCP Serverは既存のstdioに加えて、ストリーミング可能なHTTPトランスポートを提供するようになりました。

### 人間参加型ワークフローの改善
反復的な入力を必要とする人間参加型ワークフローをより信頼性の高いものにするため、サポートされているMCPクライアントでのエリシテーション（対話的な入力取得）機能が含まれました。

### 実験的機能
一般的なAWSタスクのための規範的なワークフローを提供する新しい実験的ツール「get_execution_plan」が追加されました。このツールはEXPERIMENTAL_AGENT_SCRIPTSフラグをtrueに設定することで有効にできます。

## 利用可能性

AWS API MCP Serverはオープンソースとして提供されており、AWS Labs GitHubリポジトリからソースコードの閲覧とダウンロードが可能です。また、Amazon ECR Public Galleryでコンテナとしてパッケージ化されたバージョンも利用可能です。

顧客は、いくつかの人気のあるMCPレジストリから、MCPと互換性のあるクライアントで使用するためのAWS API MCP Serverを構成できます。

このv1.0.0リリースは、AWS APIとの自然言語インタラクションを大幅に改善し、より使いやすく、安全で、柔軟なツールとなっています。
```

## まとめ

[AWS](https://d.hatena.ne.jp/keyword/AWS) Knowledge [MCP](https://d.hatena.ne.jp/keyword/MCP) Server の設定を試して実際に [AWS](https://d.hatena.ne.jp/keyword/AWS) 情報を検索してみました.今後 re:Invent もありますし情報をまとめたりサマリーするのにも役立ちそうな [MCP](https://d.hatena.ne.jp/keyword/MCP) サーバーだと感じました.

[« AWS API MCP Server v1.0.0 がリリースさ…](https://sadayoshi-tada.hatenablog.com/entry/2025/10/11/192241) [Bedrock Knowledge Base のベクトルストア… »](https://sadayoshi-tada.hatenablog.com/entry/2025/09/27/202726)