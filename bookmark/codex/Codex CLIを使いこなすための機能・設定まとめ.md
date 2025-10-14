---
title: "Codex CLIを使いこなすための機能・設定まとめ"
source: "https://zenn.dev/dely_jp/articles/codex-cli-matome"
author:
  - "[[Zenn]]"
published: 2025-09-02
created: 2025-10-14
description:
tags:
  - "clippings"
image: "https://res.cloudinary.com/zenn/image/upload/s--j7X6svl3--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Codex%2520CLI%25E3%2582%2592%25E4%25BD%25BF%25E3%2581%2584%25E3%2581%2593%25E3%2581%25AA%25E3%2581%2599%25E3%2581%259F%25E3%2582%2581%25E3%2581%25AE%25E6%25A9%259F%25E8%2583%25BD%25E3%2583%25BB%25E8%25A8%25AD%25E5%25AE%259A%25E3%2581%25BE%25E3%2581%25A8%25E3%2582%2581%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_34:%25E3%2583%25A9%25E3%2582%25AF%2Cx_220%2Cy_108/bo_3px_solid_rgb:d6e3ed%2Cg_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyLzM3MDYyNjhkNmMuanBlZw==%2Cr_20%2Cw_90%2Cx_92%2Cy_102/co_rgb:6e7b85%2Cg_south_west%2Cl_text:notosansjp-medium.otf_30:Kurashiru%2520Tech%2520Blog%2Cx_220%2Cy_160/bo_4px_solid_white%2Cg_south_west%2Ch_50%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2YwODYxOWQxYzkuanBlZw==%2Cr_max%2Cw_50%2Cx_139%2Cy_84/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT"
---
640

286[tech](https://zenn.dev/tech-or-idea)

## はじめに

こんにちは、ラクです！

最近、開発者の間でOpenAIの「Codex CLI」が話題になっていますね。  
Codex CLIは今年の4月に公開されたばかりの歴史の浅いツールですが、短いサイクルで改善・アップデートが続いています。

現時点ではClaude Codeに比べて未実装の機能もありますが、その差は急速に縮まりつつあります。  
ただし公式ドキュメントのようなものはなく、現時点ではリポジトリを直接見に行くしかありません。  
本記事ではリリースノートやPRを読んで、私が実際に使って便利に感じた設定や機能を紹介していきます。

## なぜ今、Codex CLIが話題なのか？

### ChatGPTのサブスクリプションで利用できるようになった

GPT-5の公開（2025年8月7日）以降、ChatGPTの有料プラン（Plus/Pro/Team）加入者は追加料金なし・定額でCodex CLIを利用できるようになりました。

Codex CLIが公開された2025年4月当初はAPI Key前提でした。  
Claude Codeと比べて機能的にも優位性は限定的でしたが、現在は使い勝手が大きく向上しています。

### Claude Codeの性能劣化

これはAnthropicの公式発表ではありませんが、「Claude Codeの挙動が以前と変わった」「性能が落ちた」という声が多く、私自身も同様の印象を持っています。  
こうした背景から、Claude CodeからCodex CLIへの移行を検討する開発者が増えているのをX上でよく見かけます。

## Codex CLIの基本設定

### インストール方法

公式ではnpm、もしくはHomebrewでインストールする方法が案内されています。

```shell
npm install -g @openai/codex
```

```shell
brew install codex
```

### AGENTS.mdで日本語対応を設定する

Codexでは、 `~/.codex/AGENTS.md` に共通プロンプトを記述すると、すべての会話に自動で読み込まれます  
日本語での応答をデフォルトにしたい場合は、以下のような短い指示を書いておくのがおすすめです。

```shell
日本語で簡潔かつ丁寧に回答してください
```

AGENTS.mdはCodexをはじめ、CursorやGitHub Copilot,Gemini CLI, Roo Codeなど多くのAIツールが共通して利用することができるCustom Instructionsのファイル名です(Claude Codeも早く対応して欲しい....)

### 設定ファイル: ~/.codex/config.toml

Codex CLIの設定は、基本的に `~/.codex/config.toml` ファイルで行います。  
当初Node.jsで実装されていたCodex CLIがRustでスクラッチから再実装された経緯があり、その影響で設定ファイルにはTOML形式が採用されているようです。

以下は私が実際に使用している設定例です。  
modelの指定で明示的に"gpt-5-codex"を使用するようにしています。

```toml
model = "gpt-5-codex"
model_reasoning_effort = "high"
hide_agent_reasoning = true
network_access = true

notify = ["bash", "-lc", "afplay /System/Library/Sounds/Ping.aiff"]

[tools]
web_search = true

[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp@latest"]

[mcp_servers.figma]
command = "npx"
args = ["-y", "mcp-remote", "http://127.0.0.1:3845/sse"]
```

また、 `config.toml` に記述した設定は、環境変数や実行時の引数で上書きできます。  
優先順位は次のとおりです。

1. `--config key=value` （コマンドライン引数）
2. 環境変数
3. `config.toml` の設定値

## 便利な機能と設定詳説

### GPT-5のreasoningを常にhighにする

次の設定で常に high の推論モードを有効化します。  
デフォルトではmediumが指定されていますが、highにするのがこの記事の中でも一番おすすめしたい設定です！

```toml
model_reasoning_effort = "high"
```

### Notify（Claude Hooks相当）で音を鳴らす

タスク完了時などに通知を受け取ることができます。  
macOSでサウンドを鳴らす設定例は以下のとおりです。

```toml
# config.toml
notify = ["bash", "-lc", "afplay /System/Library/Sounds/Ping.aiff"]
```

より高度なデスクトップ通知は、 [@laiso](https://x.com/laiso) さんの「 [新Codex CLIの使い方](https://blog.lai.so/codex-rs-intro/) 」が参考になります。

### MCPを設定する

Claude Code同様にMCPに対応していますが、専用のJSONではなくCodex CLI自体の設定ファイルと同じ `config.toml` に記述します。  
設定例は次のとおりです。

```toml
[mcp_servers.context7]
command = "npx"
args = ["-y", "@upstash/context7-mcp@latest"]

[mcp_servers.figma]
command = "npx"
args = ["-y", "mcp-remote", "http://127.0.0.1:3845/sse"]
```

現時点のCodex CLIは stdio 通信のみサポートのため、SSEのMCPサーバーは直接利用できません。  
`mcp-proxy` や `mcp-remote` などのラッパーを挟む方法が公式に案内されています。  
実際、FigmaなどリモートMCPが必要なものは `mcp-remote` を使用しています。

> Defines the list of MCP servers that Codex can consult for tool use. Currently, only servers that are launched by executing a program that communicate over stdio are supported. For servers that use the SSE transport, consider an adapter like mcp-proxy.

### Web検索ツールを有効にする

Codexには標準でWeb検索機能が組み込まれており、最新の情報に基づいた回答を生成させることができます。  
デフォルトでは無効になっていますが、以下の設定を追加することで有効化できます。

```toml
[tools]
web_search = true
```

![search.png](https://res.cloudinary.com/zenn/image/fetch/s--YiCd0sf_--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/1497503eafd3b43edb4bbe3c.png%3Fsha%3Ded68170697f63cd4f7ea2c3cafdca1fd6777edcf)

### Custom Prompts（Claudeのスラッシュコマンド相当）を作る

頻繁に使うプロンプトをカスタムコマンドとして登録しておく機能です。

プロジェクト配下の `.codex/prompts/` に置いても読み込まれず、 `~/.codex/prompts/` 配下に置く必要があります。  
今後に期待です！

実際にClaude Codeでも使用していたリファクタリング用のslash commandをCodexに移植しました。  
以下のように `/` を入力した後にファイル名が表示され、実行できるようになります。

![](https://res.cloudinary.com/zenn/image/fetch/s--405gT8tm--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/672d402124c1ae83b254b451.png%3Fsha%3Dda0f25ea20fe41ad63d9ac5c1102620ed95b22b3)

### reasoningログの非表示

Codexは回答生成前に内部の思考過程を示す「reasoning」イベントを断続的に出力します。  
TUI表示で冗長に感じる場合は、次の設定で非表示にできます。

```toml
# config.toml
hide_agent_reasoning = true  # 既定値は false
```

### @ でファイルをメンションする

プロンプト内で `@path/to/file` と書くと、そのファイル内容を明示的に指定することができます。  
![](https://res.cloudinary.com/zenn/image/fetch/s--M3XoPboc--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/6f06b4750b1d23eedd371a0e.png%3Fsha%3D3caf696ee1ff29dd797e4404265c7bf282bce71e)

### CodexのVSCode拡張

CodexのVS Code拡張は2025年8月27日に公開されました。  
[developers.openai.com/codex/ide](https://developers.openai.com/codex/ide)

OpenAI公式のVS Code拡張（ChatGPT Desktop連携）は以前から存在していましたが、そこにCodex CLI対応が加わった形です。

CLIをそのまま使用するときとの違いとして、以下の点が挙げられます。

- VS Codeで開いているファイルを参照できるため、 `@` でファイル名を指定しなくても雑にプロンプトを投げることができる
- Codex Cloudへのタスクを流すことができる

![](https://res.cloudinary.com/zenn/image/fetch/s--iO1golUV--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_1200/https://storage.googleapis.com/zenn-user-upload/deployed-images/9df6923ee5504f5138acf965.png%3Fsha%3D1d0def7a718d2f034e7b3ddfd7b4e1f3993fb7fc)

## 主要コマンド解説

| コマンド | Claude Code相当 | 説明 |
| --- | --- | --- |
| `/new` | `/clear` | 現在のセッションを破棄し、新しいセッションを開始します |
| `/mcp` | `/mcp` | 現在のセッションで有効になっているMCPを表示します |
| `/compact` | `/compact` | 長くなった対話履歴をモデルに要約させ、トークンを節約します |

## おまけ：Codex CLIの制限について

- Plus / Business（Team）/ Enterprise / Education
	- ローカルタスク: 平均的なユーザーは、週の制限内で 5 時間あたり 30〜150 メッセージ。
- Pro
	- ローカルタスク: 平均的なユーザーは、週の制限内で 5 時間あたり 300〜1,500 メッセージ。

OpenAIでCodexをリードする Alexander Embiricos 氏のポストによれば、これは現時点の利用状況における p25〜p75 の目安とのことです。  
制限に関しては今後の利用状況に応じて変更される可能性があると思われます。

640

286