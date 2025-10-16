---
title: "🚀 Claude Code × Serena MCP：もうバージョンダウンしなくても良いのか...?"
source: "https://zenn.dev/studio/articles/431afa748fbed1"
author:
  - "[[Zenn]]"
published: 2025-07-31
created: 2025-10-15
description:
tags:
  - "clippings"
image: "https://res.cloudinary.com/zenn/image/upload/s--7MIe7p_---/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:%2520%2520Claude%2520Code%2520%25C3%2597%2520Serena%2520MCP%25EF%25BC%259A%25E3%2582%2582%25E3%2581%2586%25E3%2583%2590%25E3%2583%25BC%25E3%2582%25B8%25E3%2583%25A7%25E3%2583%25B3%25E3%2583%2580%25E3%2582%25A6%25E3%2583%25B3%25E3%2581%2597%25E3%2581%25AA%25E3%2581%258F%25E3%2581%25A6%25E3%2582%2582%25E8%2589%25AF%25E3%2581%2584%25E3%2581%25AE%25E3%2581%258B...%253F%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:wadakatu%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2UwYmNhNWZkODEuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:Studio%2520Tech%2520Blog%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzFkYWY3MjZjMWQuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT"
---
310

101

## 📖 はじめに - Claude Codeのパフォーマンス低下問題

最近、X(Twitter)でClaude Codeのパフォーマンスに関する以下のような投稿を多く見かけます。

- Claude Codeの応答精度が低下している
- Claude Codeの出力品質に不満がある
- 最近のClaude Codeの動作が期待に沿わない

GitHubでも以下のようなissueが立てられて、Claude Codeのバージョンダウンしようみたいな風潮があります。

実際に使用していると、CLAUDE.mdに記載された指示の見落としや、プロジェクト構造の把握精度の低下を感じることがあります。  
しかし、バージョンダウンすると最新機能の `/agents` や `plan mode` が使用できなくなるため、最新版を使いながらパフォーマンスを向上させる方法を模索していました。

**そこで見つけたのが「Serena MCP」です。**

## 🎯 Serena MCPとは？

Semantic Retrieval & Editing noetic agentの略で「Serena」です。  
日本語訳すると「セマンティック検索・編集型の知的エージェント」みたいな感じです。  
ライブラリの名前ってオシャレなの多いですよね〜

Serenaは、プロジェクトのコンテキストを効率的に管理し、Claude Codeの理解力を向上させるMCP（Model Context Protocol）サーバーです。  
プロジェクト構造をセマンティックに分析し、必要な情報を適切にClaude Codeに提供することで、より正確な出力を実現するそうです。

Serena自体の詳しい説明についてはGitHubリポジトリのREADMEを参照してください

## 🛠 Mac環境での環境構築

Serenaには複数のインストール方法があります。あなたの環境に合わせて選択してください。

### 📋 前提条件

- macOS（Intel/Apple Silicon対応）
- Claude Code がインストール済み
- Homebrew がインストール済み

### 方法1: uvxを使った直接実行（推奨）

最も簡単な方法です。事前のインストールが不要で、常に最新版を使用できます。  
自分もこの方法を使っています。手頃で良い感じです。

#### Step 1: uvのインストール

```bash
# Homebrewでuvをインストール
brew install uv
```

#### Step 2: Claude Codeへの登録

プロジェクトディレクトリで以下を実行：

```bash
claude mcp add serena -- uvx --from git+https://github.com/oraios/serena serena-mcp-server --context ide-assistant --project $(pwd)
```

### 方法2: ローカルインストール

より細かい制御が必要な場合や、オフライン環境で使用する場合に適しています。

#### Step 1: Serenaのクローンとインストール

```bash
# 作業ディレクトリを作成
mkdir -p ~/Developer/mcp && cd ~/Developer/mcp

# Serenaをクローン
git clone https://github.com/oraios/serena
cd serena

# mcpサーバー起動
uv run serena-mcp-server
```

#### Step 2: Claude Codeへの登録

```bash
# プロジェクトディレクトリで実行
claude mcp add serena -- uv run --directory ~/Developer/mcp/serena serena-mcp-server --project $(pwd)
```

### 方法3: Docker(⚠️)

隔離された環境で実行したい場合はDockerを使いましょう  
**注意**: Docker版はまだ本番Readyではなく、いくつかの制限があるそうです。

```bash
# Dockerイメージを使用
docker run --rm -i --network host \
  -v ~/Projects:/workspaces/projects \
  ghcr.io/oraios/serena:latest \
  serena-mcp-server --transport stdio
```

### 方法4: SSEモード（トラブルシューティング用）

標準のstdio通信で問題が発生する場合：

```bash
# SSEモードでサーバーを起動
uv run --directory ~/Developer/mcp/serena serena-mcp-server --transport sse --port 9121

# Claude Codeの設定でSSE接続を設定
# http://localhost:9121/sse を指定
\`\`\`動
uv run --directory ~/Developer/mcp/serena serena-mcp-server --transport sse --port 9121
```

### ✅ 動作確認

```bash
# MCPサーバーの状態確認
claude mcp list

# 出力例：
# Available MCP servers:
# - serena (connected) ✅
```

Serena MCPが起動すると、ブラウザで `http://localhost:24282/dashboard/index.html` が自動で開くはずです。

## 🎮 実際の使い方

### 1️⃣ プロジェクトの初期化

新しいプロジェクトでSerenaを使い始める際：

```bash
# プロジェクトディレクトリで
cd /path/to/your/project

# Claude Codeを起動
claude
```

---

Claude Code上で、 `/mcp` を入力して、Serena MCPの起動状況を確認

以下のような表示なればOK  
![](https://storage.googleapis.com/zenn-user-upload/e37bdeb4188d-20250731.png)

---

Claude Codeのチャットで以下のコマンドを実行：

```
/mcp__serena__initial_instructions
```

または自然言語で：

```
read Serena's initial instructions
```

---

これにより、Serenaがプロジェクトをスキャンし、コードの構造を理解します。  
このスキャンは１回走ると、.serenaディレクトリを新規作成します  
そのディレクトリの中に、project構造などのmemoryをmd形式で複数作成してくれます。（CLAUDE.mdみたいな感じかなと思ってます）

## 🔧 設定のカスタマイズ

### グローバル設定

`~/.serena/serena_config.yml` でグローバルな設定が可能：

### プロジェクト固有の設定

プロジェクトディレクトリに `.serena/project.yml` を作成：

## 🚀 さらなる活用

### 他のツールとの連携

SerenaはClaude Code以外にも様々なMCPクライアントと連携できるので、Claude Code以外に恩恵を得られるはずです。

- **IDE統合**: VSCode, Cursor, IntelliJ
- **CLI ツール**: Cline, Roo Code, Goose
- **その他**: Windsurf, Agno

### コミュニティとサポート

- [GitHub Repository](https://github.com/oraios/serena)
- [Changelog](https://github.com/oraios/serena/blob/main/CHANGELOG.md)
- [Roadmap](https://github.com/oraios/serena/blob/main/ROADMAP.md)

## 🎯 まとめ

Serenaを使ってまだ3日くらいですが、かなりClaude Codeの出力精度が上がっている気がして、実装中の不満が格段に減りました。（個人の感想です。）  
AI系のツールって最初の数回使って期待値を下回ったらすぐ使わなくなって、次のツールに移るみたいなことが多いんですが、Serenaは期待値を上回る結果でこれからも使い続ける予定です！

現状のClaude Codeに満足していない方、Serenaを導入してみませんか？  
他にも良いツールあれば教えてください！

Serenaに興味が出てきた方は、次はこの記事を読んでみるのおすすめです！  
より実践的にSerenaを使う方法が書いてあり、勉強になりました！

310

101