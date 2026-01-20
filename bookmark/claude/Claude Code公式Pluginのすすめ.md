---
title: "Claude Code公式Pluginのすすめ"
source: "https://zenn.dev/modokkin/articles/zenn-2025-12-03-tech-claude-code-plugins?redirected=1"
author:
  - "[[Zenn]]"
published: 2025-12-03
created: 2026-01-20
description:
tags:
  - "clippings"
image: "https://res.cloudinary.com/zenn/image/upload/s--GPdT_1ss--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Claude%2520Code%25E5%2585%25AC%25E5%25BC%258FPlugin%25E3%2581%25AE%25E3%2581%2599%25E3%2581%2599%25E3%2582%2581%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2581%25AF%25E3%2581%25B6%25E3%2581%25A1%25E3%2582%2593%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2Q4ZDhlMmQyZTAuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT"
---
186

93[tech](https://zenn.dev/tech-or-idea)

この記事は tacoms Advent Calendar 2025 3日目の記事です。

こんにちは、株式会社 tacoms で SRE をやっている はぶちん ([@modokkin](https://x.com/modokkin)) です。  
気付けばあっという間に12月に入りましたね。ガーデナーにとってはそろそろ冬越しの準備が必要な時期ですが、手入れが楽になる一方で、しばらく遠のくと思うと寂しくもあります。  
寒くなってきたので、みなさまお身体にはお気をつけください。

さて、今回はClaude Codeの公式Pluginについて紹介します。Plugin機能については公式ブログなどで知っている方も多いと思いますが、公式リポジトリで公開されているPluginについてはざっとウェブを調べても解説している記事は見つかりませんでした。  
ちょっと試してみたら、さすが公式と思える模範的なPluginが揃っていたので、紹介したいと思います。  
すでにPluginを自前で用意されている方には釈迦に説法かもしれませんが、これから利用する方には有益な内容だと思いますので宜しければご覧ください。

## 公式Pluginとは

Claude Codeには、Anthropicが公式に提供しているPluginがあります。GitHubリポジトリで公開されており、簡単にインストールできます。  
  
Plugin機能自体の解説記事は多く見かけますが、公式Pluginの具体的な使い方を解説している記事は見当たらなかったのでまとめてみます。  

2025/12/03時点で、以下のPluginが公開されています。

| Plugin名 | カテゴリ | 説明 |
| --- | --- | --- |
| agent-sdk-dev | development | Agent SDKプロジェクト向けの開発キット |
| claude-opus-4-5-migration | development | Sonnet 4.xやOpus 4.1からOpus 4.5への移行支援 |
| code-review | productivity | 複数の専門エージェントによるPRの自動レビュー |
| commit-commands | productivity | コミット、プッシュ、PR作成などのGitワークフロー用コマンド |
| explanatory-output-style | learning | 実装選択やコードベースパターンに関する教育的な解説を追加 |
| feature-dev | development | 専門エージェントによる包括的な機能開発ワークフロー |
| frontend-design | development | 本番環境品質の独自性のあるフロントエンドインターフェースを作成 |
| hookify | productivity | 望ましくない動作を防ぐカスタムフックを簡単に作成 |
| learning-output-style | learning | 意味のあるコード貢献を促すインタラクティブ学習モード |
| plugin-dev | development | Claude Code Plugin開発のための包括的なツールキット |
| pr-review-toolkit | productivity | コメント、テスト、サイレントエラーなどを専門とするPRレビューエージェント群 |
| ralph-wiggum | development | 反復開発のための自己参照型AIループ |
| security-guidance | security | 潜在的なセキュリティリスクを警告するセキュリティリマインダーフック |

本記事では、このうち私が実際にインストールしたPluginについて紹介します。

## Pluginのインストール方法

### 1\. MarketplaceをClaude Codeに登録

```bash
▐▛███▜▌   Claude Code v2.0.55
▝▜█████▛▘  Opus 4.5 · Claude Max
  ▘▘ ▝▝    ~/work/github.com/habuchin

> /plugin marketplace add anthropics/claude-code
  ⎿  Successfully added marketplace: claude-code-plugins
```

### 2\. MarketplaceからPluginをインストール

対話的なUIが表示され、簡単にインストールできます。

```bash
# Browse and install plugins を選択
> /plugin
╭──────────────────────────────────────────────────────────────────────────╮
│ Plugins                                                                  │
│                                                                          │
│ ❯ 1. Browse and install plugins                                          │
│   2. Manage and uninstall plugins                                        │
│   3. Add marketplace                                                     │
│   4. Manage marketplaces                                                 │
│   5. View installation status                                            │
╰──────────────────────────────────────────────────────────────────────────╯
   Press ↑↓ to navigate · Enter to select · Esc to exit

# 以下のようにMarketplaceに含まれるPlugin一覧が表示されるので、
# 対象を選択して半角スペースを入力してチェックし、iキーを入力するとインストールされます。
╭─────────────────────────────────────────────────────────────────────────────╮
│ claude-code-plugins › Install plugins                                       │
│                                                                             │
│   ◯ agent-sdk-dev [development]                                             │
│     Development kit for working with the Claude Agent SDK                   │
│                                                                             │
│   ◉ claude-opus-4-5-migration [development]                                 │
│     Migrate your code and prompts from Sonnet 4.x and Opus 4.... · v1.0.0   │
│                                                                             │
│   ◉ code-review [productivity]                                              │
│     Automated code review for pull requests using multiple sp... · v1.0.0   │
│                                                                             │
│   ◉ commit-commands [productivity]                                          │
│     Commands for git commit workflows including commit, push,... · v1.0.0   │
│                                                                             │
│   ◯ explanatory-output-style [learning]                                     │
│     Adds educational insights about implementation choices an... · v1.0.0   │
│                                                                             │
│   ◉ feature-dev [development]                                               │
│     Comprehensive feature development workflow with specializ... · v1.0.0   │
│                                                                             │
│   ◉ frontend-design [development]                                           │
│     Create distinctive, production-grade frontend interfaces ... · v1.0.0   │
│                                                                             │
│   ◯ hookify [productivity]                                                  │
│     Easily create custom hooks to prevent unwanted behaviors ... · v0.1.0   │
│                                                                             │
│   ◯ learning-output-style [learning]                                        │
│     Interactive learning mode that requests meaningful code c... · v1.0.0   │
│                                                                             │
│   ◉ plugin-dev [development]                                                │
│     Comprehensive toolkit for developing Claude Code plugins.... · v0.1.0   │
│                                                                             │
│   ◉ pr-review-toolkit [productivity]                                        │
│     Comprehensive PR review agents specializing in comments, ... · v1.0.0   │
│                                                                             │
│   ◉ ralph-wiggum [development]                                              │
│     Interactive self-referential AI loops for iterative devel... · v1.0.0   │
│                                                                             │
│ ❯ ◉ security-guidance [security]                                            │
│     Security reminder hook that warns about potential securit... · v1.0.0   │
╰─────────────────────────────────────────────────────────────────────────────╯
   Press i to install · Space: (de)select ·

# インストールが成功すると以下のようなメッセージが表示されるのでClaudeCodeを再起動します。
> /plugin
  ⎿  ✓ Installed 9 plugins. Restart Claude Code to load new plugins.
```

## 今回紹介するPlugin

### 1\. commit-commands

よく使うGit操作を自動化する3つのスラッシュコマンドです。

| コマンド | 機能 |
| --- | --- |
| `/commit` | 変更内容を分析してコミットメッセージを自動生成 |
| `/commit-push-pr` | ブランチ作成→コミット→プッシュ→PR作成を一括実行 |
| `/clean_gone` | リモートで削除されたブランチのローカル残存を自動削除 |

### 2\. code-review

PRを自動でコードレビューするPluginです。複数のエージェントを並列で起動し、信頼度スコアリングで誤検知を除外します。

#### 5つの専門エージェント

| エージェント | 役割 |
| --- | --- |
| CLAUDE.md遵守チェック | 変更がCLAUDE.mdのガイドラインに準拠しているか監査 |
| バグ検出 | 変更内容に限定した明らかなバグを浅くスキャン |
| 履歴分析 | git blameを活用し、過去の変更履歴から問題を検出 |
| 過去PRコメント分析 | 関連ファイルの過去PRコメントを確認し、適用可能な指摘を検出 |
| コードコメント遵守チェック | コード内のコメントに書かれたガイダンスへの準拠を確認 |

各検出事項は0〜100点で独立採点され、デフォルトの信頼度閾値80以上の問題のみがレポートされます。

| コマンド | 機能 |
| --- | --- |
| `/code-review:code-review` | PRを自動でコードレビュー |

### 3\. pr-review-toolkit

PRの品質を多角的に分析する6つの専門エージェントの集合です。code-reviewとは異なり、より細かい観点でレビューします。

#### 6つのエージェント

| エージェント | 役割 |
| --- | --- |
| comment-analyzer | コメントとドキュメントの正確性を検証 |
| pr-test-analyzer | テストカバレッジの質を分析 |
| silent-failure-hunter | エラーハンドリングの不備を追跡 |
| type-design-analyzer | 型定義の品質を1〜10スケールで評価 |
| code-reviewer | 一般的なコード品質をチェック |
| code-simplifier | 複雑性削減、可読性向上に焦点 |

| コマンド | 機能 |
| --- | --- |
| `/pr-review-toolkit:review-pr` | 6つのエージェントでPRを多角的にレビュー |

### 4\. feature-dev

新機能開発を体系的に進めるための7段階のワークフローを提供します。

#### 7つのフェーズ

1. Discovery: 機能要件を明確化し、解決する問題を定義
2. Codebase Exploration: 複数のエージェントが並行して既存コードを分析
3. Clarifying Questions: 曖昧な要件を洗い出し、エッジケースを確認
4. Architecture Design: 3つの異なるアプローチ案を提示
5. Implementation: 承認後、設計に従ってコードを実装
6. Quality Review: 複数のレビュワーが並行レビュー
7. Summary: 実装内容と推奨される次のステップをまとめ

| コマンド | 機能 |
| --- | --- |
| `/feature-dev:feature-dev <説明>` | 7段階のワークフローで新機能開発を開始 |

### 5\. frontend-design

「AIっぽい」ジェネリックなデザインを避け、本番環境対応の高品質なフロントエンドインターフェースを生成するPluginです。  
少し前に [こちら](https://www.claude.com/blog/improving-frontend-design-through-skills) の記事が話題になりましたね。

#### 特徴

- 大胆な美的選択
- 独特のタイポグラフィと色彩設計
- 高度なアニメーションと視覚効果
- 文脈に応じた実装

Claudeが自動的にこのスキルを検出・活用するため、明示的に指定する必要はありません。

### 6\. claude-opus-4-5-migration

アプリケーションコード内でClaude APIを直接呼び出している場合に、Opus 4.5への移行を支援するPluginです。  
コード内に `claude-sonnet-4-*` や `claude-opus-*` などのモデル文字列を使用している場合が対象です。  
Anthropic APIクライアント（ `messages.create` 等）を使用している場合も対象です。

Opus 4.5の振る舞いが変わったことで、以前のモデルだと必要だった指示が非推奨になっていたりするようで、その移行を補助してくれるようです。

#### 機能

- 自動マイグレーション: 既存のコード・プロンプトをOpus 4.5互換に自動更新
- モデル文字列やベータヘッダーなどの設定を自動処理
- 移行後の問題発生時にも追加調整が可能

| コマンド | 機能 |
| --- | --- |
| `Migrate my codebase to Opus 4.5` | Opus 4.5への移行を開始（自然言語で指示） |

### 7\. ralph-wiggum

AI支援の開発ループを自動化するユニークなPluginです。「Ralph is a Bash loop」という考え方に基づいています。  
AIエージェントが同じプロンプトを繰り返し実行しながら、生成されたファイルと履歴を参照して自律的に改善していく仕組みです。

#### コマンド

| コマンド | 機能 |
| --- | --- |
| `/ralph-loop "<タスク説明>"` | ループを開始 |
| `/cancel-ralph` | 実行中のループを停止 |

オプション: `--max-iterations` （制限回数）、 `--completion-promise` （完了を示すテキスト）

### 8\. security-guidance

ファイル編集時にセキュリティリスクを検出するフックPluginです。  
開発中にClaudeがこの機能で指摘されているのを何度か見かけたのですが、人間が気づきを与える手間を軽減してくれている感じがしました。

#### 機能

- ファイル編集時に自動でセキュリティチェック
- 潜在的なセキュリティリスクを警告

## Pluginを使ってみた

### commit-commandsでコミットしてみる

/commitと入力すると以下のように候補が表示されます。

```bash
───────────────────────────────────────────────────────────────────────────────
> /commit
───────────────────────────────────────────────────────────────────────────────
  /commit-commands:commit-push-pr
      Commit, push, and open a PR
      (plugin:commit-commands@claude-code-plugins)
  /commit-commands:commit
      Create a git commit (plugin:commit-commands@claude-code-plugins)
  /commit-commands:clean_gone
      Cleans up all git branches marked as [gone] (branches that have been
      deleted on the remote but still exist locally), including removing
      associated worktrees. (plugin:commit-commands@claude-code-plugins)
```

パッと見ただけでは、 `/commit` なんてわざわざコマンド化しなくても良いじゃん。と思うかもしれないのですがこのスラッシュコマンドの中身を見てみると必要性が分かると思います。  
`allowed-tools:` で許可するコマンドを制限し、 `## Context` で実行する内容を記述し、 `## Your task` で余計な出力をしないように注意書きが書かれています。  
すでにスラッシュコマンドを独自に作っている方もおられると思いますが、こういうClaudeCodeのコツみたいなものを汎用化してくれるのはありがたいですね。

[plugins/commit-commands/commands/commit.md](https://github.com/anthropics/claude-code/blob/main/plugins/commit-commands/commands/commit.md)

```bash
---
allowed-tools: Bash(git add:*), Bash(git status:*), Bash(git commit:*)
description: Create a git commit
---

## Context

- Current git status: !\`git status\`
- Current git diff (staged and unstaged changes): !\`git diff HEAD\`
- Current branch: !\`git branch --show-current\`
- Recent commits: !\`git log --oneline -10\`

## Your task

Based on the above changes, create a single git commit.

You have the capability to call multiple tools in a single response. Stage and create the commit using a single message. Do not use any other tools or do anything else. Do not send any other text or messages besides these tool calls.
```

### claude-opus-4-5-migrationをClaudeCode向けに使ってみた

自分はClaudeCode向けのドキュメントなどをマイグレーションしてくれるものだと勘違いしていたのですが、実行してみたら用途が違うことに気付きました。

> Opus 4.5マイグレーションは、アプリケーションコード内でClaude APIを直接呼び出している場合（例：claude-sonnet-4-20250514などのモデル文字列をAPIリクエストで使用）に必要なものです。

でも、よく考えたらルールにそって修正できるんじゃ？（のび太の閃いた顔）

```bash
> 応用してClaudeCode向けのドキュメントをレビューできませんか？

⏺ なるほど！Opus 4.5の行動特性に基づいて、Claude
  Code向けドキュメント（CLAUDE.md等）をレビューするということですね。良いアイデアです。

  関連ドキュメントを確認させてください。
```

で、結果的に以下のような指摘をしてくれました。

```bash
Opus 4.5向けCLAUDE.mdレビュー結果

  分析対象

  - /AGENTS.md（ルート）
  - /frontend/AGENTS.md

  ---
  1. ツール過剰発動リスク（高）

  Opus 4.5はシステムプロンプトに敏感で、従来モデルで「未発動防止」のために使われた強い表現が「過剰発動」を引き起
  こす可能性があります。

  検出パターン（ルートAGENTS.md）

  | 行        | 現在の表現                    | 推奨変更          |
  |----------|--------------------------|---------------|
  | 57       | [CRITICAL] Issue操作の絶対ルール | Issue操作ルール    |
  | 118      | [重要] 最重要ルール              | TDDルール        |
  | 122      | 絶対ルール                    | 基本ルール         |
  | 125      | やってはならない                 | 避ける           |
  | 159, 164 | // [CRITICAL]            | // [注意]       |
  | 426      | [重要] 最重要: Makefile利用の徹底  | Makefile利用ガイド |
  | 428      | 必ずMakefileを使用すること        | Makefileを使用する |

  検出パターン（frontend/AGENTS.md）

  このファイルはMUST:とNEVER:を多用していますが、スタイルガイドとしては適切です。ただし、Opus
  4.5では各項目を過剰に厳格に適用する可能性があります。

  推奨: 重要度の低い項目はSHOULD:に格下げを検討

  ---
  2. 過剰エンジニアリング防止（中）

  Opus 4.5は余分なファイル作成、不要な抽象化、依頼されていない柔軟性を追加する傾向があります。

  現状評価

  AGENTS.mdには以下の良い指針があります：
  - TDDの「最小の実装」原則（line 131-132）
  - エビデンスベース判断（line 179-268）

  推奨追加

  以下のスニペットをAGENTS.mdに追加することを推奨：

  ### 過剰エンジニアリング防止

  依頼された内容のみを実装する：
  - 依頼されていないファイルを作成しない
  - 不要な抽象化を追加しない
  - 「将来のため」の柔軟性を追加しない
  - 同様の3行のコードは、早計な抽象化より優先

  ---
  3. コード探索（良好）

  Opus 4.5はコードを読まずに解決策を提案する傾向がありますが、現在のドキュメントは適切に対処しています。

  現状の良い点

  - エビデンス確認プロセス（line
  187-205）で「関連ドキュメントを読んだか？」「既存コードでパターンを探したか？」を要求
  - line 418-424の「常にテストファーストで作業する」指針

  評価: 追加対応不要

  ---
  4. 「think」感度（低リスク）

  Extended Thinkingが無効の場合、Opus 4.5は「think」という単語に敏感です。

  現状評価

  日本語ドキュメントのため、「think」の直接的な使用はありません。英語部分（コメント等）でも問題なし。

  評価: 対応不要

  ---
  推奨アクション一覧

  優先度高（過剰発動防止）

  1. [CRITICAL]を[注意]または削除
  2. 絶対ルールを基本ルールに
  3. 必ず〜することを〜するに
  4. やってはならないを避けるに

  優先度中（過剰エンジニアリング防止）

  5. 過剰エンジニアリング防止セクションを追加

  適用判断

  変更を適用しますか？

  - A: 全て適用（推奨）
  - B: 優先度高のみ適用
  - C: 現状維持（問題が発生したら対応）
```

ちなみに出力にAGENTS.mdと表示されていますが、これはこのプロジェクトでCLAUDE.mdをAGENTS.mdのシンボリックリンクにしているためです。

APIとして遵守すべき場合と、ClaudeCodeで遵守してほしい項目はちょっと違うかもしれませんが、濃淡をつける塩梅のチェックなんかには良いかもしれません。  
今回は一旦指摘を受け入れてみて、適宜調整していくことにしました。

### pr-review-toolkitでPRレビューしてみる

試しに個人開発しているリポジトリで、ほぼノールックで作ったTerraformのワークフロー更新PRに対して使ってみました。

結果はCritical、Important、Suggestionsに分類され、以下のような指摘がありました。

- tfstateがローカルバックエンドでコミットされるリスク
	- .gitignore に漏れがってtfstateがコミットする状態になっていた
- planとapplyジョブのenvironment不一致
	- 単純なミスを指摘してくれた
- GitHub Actions変数の検証がない（Silent Failure）
	- ClaudeCodeの開発時に実施済みだったので、私のプロジェクトでは過剰な指摘だった
- ドキュメントとの整合性の問題
	- 今回の変更がドキュメントに反映されていなかった漏れを見つけてくれた

さらに良い点（Positive Observations）も指摘されました。

- Workload Identity Federationを使用している（セキュリティベストプラクティス）
- GCSバケット設定が適切

人間のレビュワーが見落としがちな点も含めて網羅的にチェックしてくれるので、マージ前の最終確認として有用だと感じました。  
複数エージェントを使って割とガッツリレビューしてくれるので、作業完了時などここぞというタイミングで使うと良さそうです。

ちなみに役割が似ている `/code-review:code-review` はレビューの観点が異なります。CLAUDE.md準拠やgit履歴を踏まえたバグ検出など、プロジェクト固有のルール遵守に焦点を当てているようです。精度の高い指摘のみをフィードバックするなど、アウトプットの方針も異なります。一方、pr-review-toolkitはテストカバレッジや型設計、エラーハンドリングなどコード品質面を多角的にチェックします。（ちょっと今回使った対象があまり良くなかったかもしれません）

## まとめ

Claude Codeの公式Pluginは、すぐにClaudeCodeのプラクティスを実践でき、開発ワークフローを効率化してくれます。

特に以下の点が印象的でした。

- commit-commandsのような単純なコマンドでも、 `allowed-tools` や `## Your task` で適切に制約することで、Claudeの動作を正確にコントロールできる
- pr-review-toolkitのように複数の専門エージェントを並列実行する設計により、網羅的かつ精度の高いレビューが可能

また、このPluginの作り方を参考にすることで、独自のPluginやプロンプトを作る時の参考になりそうでした。（どちらかというとこっちの意味が強いのかもしれませんが）

まだPluginを使ったことがない方は、ぜひこの機会に試してみてください。

最後までお読み頂きありがとうございます。この記事がどなたかのお役に立てば嬉しいです。

186

93