---
title: "Anthropic公式「skill-creator」完全ガイド 〜 Skillsを作れるSkillsで脱・ゼロから設計〜"
source: "https://zenn.dev/yamato_snow/articles/30e8dcf9490c88"
author:
  - "[[Zenn]]"
published: 2025-12-24
created: 2026-03-03
description:
tags:
  - "clippings"
image: "https://res.cloudinary.com/zenn/image/upload/s--FXsVbjU3--/c_fit%2Cg_north_west%2Cl_text:notosansjp-medium.otf_55:Anthropic%25E5%2585%25AC%25E5%25BC%258F%25E3%2580%258Cskill-creator%25E3%2580%258D%25E5%25AE%258C%25E5%2585%25A8%25E3%2582%25AC%25E3%2582%25A4%25E3%2583%2589%2520%25E3%2580%259C%2520Skills%25E3%2582%2592%25E4%25BD%259C%25E3%2582%258C%25E3%2582%258BSkills%25E3%2581%25A7%25E8%2584%25B1%25E3%2583%25BB%25E3%2582%25BC%25E3%2583%25AD%25E3%2581%258B%25E3%2582%2589%25E8%25A8%25AD%25E8%25A8%2588%25E3%2580%259C%2Cw_1010%2Cx_90%2Cy_100/g_south_west%2Cl_text:notosansjp-medium.otf_37:%25E3%2582%2584%25E3%2581%25BE%25E3%2581%25A8%2Cx_203%2Cy_121/g_south_west%2Ch_90%2Cl_fetch:aHR0cHM6Ly9zdG9yYWdlLmdvb2dsZWFwaXMuY29tL3plbm4tdXNlci11cGxvYWQvYXZhdGFyL2UxMTY0YzRhNDUuanBlZw==%2Cr_max%2Cw_90%2Cx_87%2Cy_95/v1627283836/default/og-base-w1200-v2.png?_a=BACAGSGT"
---
136

84[tech](https://zenn.dev/tech-or-idea)

## はじめに

「Skillを自作したいけど、毎回ゼロからSKILL.mdを設計して詰む」

Claude Codeを使っている方なら、一度はこの壁にぶつかったことがあるのではないでしょうか。

Anthropicが公開している **skill-creator** は、まさにこの問題を解決するための「 **Skillsを作れるSkills** 」です。自然言語で対話しながら、Claudeが自動でSkillのフォルダ構成・SKILL.mdファイル・必要なスクリプトを生成してくれます。

この記事では、skill-creatorの導入から実際の使い方、そして活用時の注意点までを体系的に解説します。

### この記事で解決できる課題

- SKILL.mdの書き方がわからない
- Skillのディレクトリ構成で毎回迷う
- 作ったSkillがバリデーションエラーで動かない
- Progressive Disclosure（段階的開示）の設計原則が理解できていない

### 対象読者

- Claude Codeを既に使っている方
- Skillの存在は知っているが、うまく活用できていない方

---

## 1\. anthropics/skills リポジトリの全体像

まず、skill-creatorを理解する前に、 [anthropics/skills リポジトリ](https://github.com/anthropics/skills) の全体像を把握しましょう。

### 1.1 リポジトリの構成

```
anthropics/skills/
├── skills/              # Skill実例集（メイン）
│   ├── skill-creator/   # ★ 本記事の主題
│   ├── algorithmic-art/
│   ├── mcp-builder/
│   ├── webapp-testing/
│   └── ...
├── document-skills/     # ドキュメント系Skill
│   ├── docx/
│   ├── pdf/
│   ├── pptx/
│   └── xlsx/
├── spec/                # Agent Skills仕様書
├── template/            # Skillテンプレート
└── .claude-plugin/      # プラグイン設定
    └── marketplace.json
```

### 1.2 含まれるSkill一覧

このリポジトリには、実際に動作するSkillが多数含まれています。 **これらを参考にすることで、自分のSkillの設計に活かせます。**

| カテゴリ | Skill名 | 用途 |
| --- | --- | --- |
| **Creative** | algorithmic-art | p5.jsでジェネレーティブアート生成 |
|  | canvas-design | PNG/PDF形式のビジュアルアート作成 |
|  | slack-gif-creator | Slack用アニメーションGIF作成 |
| **Development** | mcp-builder | MCPサーバー構築ガイド |
|  | webapp-testing | Playwrightでローカルアプリテスト |
|  | artifacts-builder | React/Tailwind/shadcn-uiでArtifact構築 |
| **Enterprise** | brand-guidelines | Anthropicブランドガイドライン適用 |
|  | internal-comms | 社内コミュニケーション文書作成 |
|  | theme-factory | 10種類のプリセットテーマ適用 |
| **Meta** | **skill-creator** | **Skill作成支援（本記事の主題）** |
| **Document** | docx/pdf/pptx/xlsx | ドキュメント作成・編集 |

### 1.3 2つのプラグイン

リポジトリは2つのプラグインに分かれています。

| プラグイン名 | 内容 | ライセンス |
| --- | --- | --- |
| `example-skills` | Creative/Development/Enterprise/Metaの各Skill | Apache 2.0（オープンソース） |
| `document-skills` | docx/pdf/pptx/xlsx | Source-available（参照用） |

---

## 2\. Agent Skillsとは何か

### 2.1 Skillsの基本概念

Agent Skillsは、Claudeの能力を拡張するための「指示・スクリプト・リソースをまとめたフォルダ」です。

Anthropicの公式ドキュメントでは以下のように定義されています。

> Skills are folders of instructions, scripts, and resources that Claude loads dynamically to improve performance on specialized tasks.
> 
> — [Anthropic公式リポジトリ](https://github.com/anthropics/skills)

つまり、Skillsとは「特定のタスクをClaudeに繰り返し実行させるための知識パッケージ」です。

### 2.2 Skillの構造

Skillは以下のような構造を持ちます。

**ディレクトリ構成例：**

```
my-skill/
├── SKILL.md          # 必須：メタデータと指示
├── scripts/          # 任意：実行可能コード
│   └── rotate_pdf.py
├── references/       # 任意：参照ドキュメント
│   └── schema.md
└── assets/           # 任意：テンプレート等
    └── template.html
```

Skillsの設計における最も重要な原則が「Progressive Disclosure（段階的開示）」です。

これは「必要な情報を、必要なタイミングで、必要な分だけ読み込む」という設計思想です。

**なぜこの設計が重要か：**

- コンテキストウィンドウは有限なリソース
- すべての情報を最初から読み込むと、トークンを浪費する
- 必要な情報だけを段階的に読み込むことで、効率的に動作する

> Progressive disclosure is the core design principle that makes Agent Skills flexible and scalable.
> 
> — [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

---

## 3\. As-Is / To-Be 分析：従来手法との比較

skill-creatorを理解するために、まず「従来のSkill作成方法」と「skill-creatorを使った方法」を比較します。

### 3.1 As-Is：従来のSkill作成における課題

従来、Skillを作成するには主に2つのアプローチがありました。

#### パターンA：SKILL.mdを手動でゼロから作成

**課題：**

| 課題 | 詳細 |
| --- | --- |
| フロントマターの書式エラー | `name`, `description` 以外に `version` や `author` を書くとエラー |
| description文字数制限 | 200文字（APIでは1024文字）を超えるとエラー |
| ディレクトリ構成の不明確さ | scripts/references/assets の使い分けがわからない |
| Progressive Disclosureの設計ミス | 情報の配置場所を間違え、非効率なSkillになる |

#### パターンB：毎回プロンプトで詳細指示

**課題：**

| 課題 | 詳細 |
| --- | --- |
| 毎回同じ説明を繰り返す | 再利用性がない |
| プロンプトの肥大化 | 複雑なタスクほどプロンプトが長くなる |
| 品質のばらつき | プロンプトの書き方で結果が変わる |
| ナレッジの蓄積ができない | 成功パターンが個人の記憶に依存 |

### 3.2 To-Be：skill-creatorで実現できる姿

skill-creatorを使うと、以下のようなフローに変わります。

**改善点：**

| 改善項目 | Before | After |
| --- | --- | --- |
| 初期構成 | 手動でディレクトリ作成 | init\_skill.pyで自動生成 |
| フロントマター | 書式を調べながら記述 | テンプレートに沿って記述 |
| バリデーション | Claudeにインポート時にエラー発覚 | quick\_validate.pyで事前チェック |
| パッケージング | 手動でZIP作成 | package\_skill.pyで自動化 |
| ナレッジ | 個人の記憶に依存 | Skillとして組織で共有可能 |

---

## 4\. skill-creatorとは

### 4.1 skill-creatorの概要

skill-creatorは、Anthropicが公式に提供している「Skillを作成するためのSkill」です。

> The "skill-creator" skill provides interactive guidance: Claude asks about your workflow, generates the folder structure, formats the SKILL.md file, and bundles the resources you need.
> 
> — [Introducing Agent Skills](https://www.anthropic.com/news/skills)

**主な機能：**

- 対話形式でSkillの要件をヒアリング
- init\_skill.pyによるテンプレート自動生成
- quick\_validate.pyによる事前バリデーション
- package\_skill.pyによる.skillファイル作成

### 4.2 含まれるスクリプト

skill-creatorには以下の3つの主要スクリプトが含まれています。

| スクリプト | 役割 | 使用タイミング |
| --- | --- | --- |
| `init_skill.py` | Skillのテンプレートディレクトリを生成 | 新規Skill作成時 |
| `quick_validate.py` | SKILL.mdのフロントマターと構造を検証 | 編集後の確認時 |
| `package_skill.py` | Skillを.skillファイル（ZIP形式）にパッケージ | 配布・インポート時 |

### 4.3 skill-creatorの実行シーケンス

skill-creatorを使ったSkill作成の全体フローを示します。

---

## 5\. 導入方法と実際の使い方

### 5.1 Claude Codeでの導入手順

Claude CodeでSkillsを導入する方法は **3つ** あります。

#### 方法1：マーケットプレイス経由（推奨）

```
# Step 1: マーケットプレイスを登録
/plugin marketplace add anthropics/skills

# Step 2: プラグインをインストール（デフォルトはuser scope）
/plugin install example-skills@anthropic-agent-skills
/plugin install document-skills@anthropic-agent-skills
```

#### インストール時のスコープ選択

対話的にインストールする場合、以下の選択肢が表示されます：

```
> Install for you (user scope)
  Install for all collaborators on this repository (project scope)
  Install for you, in this repo only (local scope)
  Back to plugin list
```

**各スコープの違い：**

| スコープ | 説明 | 保存先 | 共有範囲 |
| --- | --- | --- | --- |
| **User scope** | 自分用、全プロジェクト共通 | `~/.claude/` | 自分のみ |
| **Project scope** | このリポジトリの全コラボレーター | `.claude/settings.json` | リポジトリをcloneした全員 |
| **Local scope** | 自分用、このリポジトリのみ | ローカル設定 | 自分のみ（Git管理外） |
| **Managed scope** | 管理者がインストール | 管理者設定 | 組織全体（変更不可） |

**スコープの選び方：**

> User scope (default): install for yourself across all projects  
> Project scope: install for all collaborators on this repository (adds to.claude/settings.json)  
> Local scope: install for yourself in this repository only (not shared with collaborators)
> 
> — [Claude Code Docs - Discover and install plugins](https://code.claude.com/docs/en/discover-plugins)

**コマンドラインでスコープを指定する場合：**

```
# User scope（デフォルト）
/plugin install example-skills@anthropic-agent-skills

# Project scope
/plugin install example-skills@anthropic-agent-skills --scope project

# Local scope
/plugin install example-skills@anthropic-agent-skills --scope local
```

#### 方法2：git cloneして手動インストール

```
# リポジトリをclone
git clone https://github.com/anthropics/skills.git

# 個人用ディレクトリにSkillをコピー（User scope相当）
mkdir -p ~/.claude/skills
cp -r skills/skills/skill-creator ~/.claude/skills/
cp -r skills/skills/mcp-builder ~/.claude/skills/

# プロジェクトローカルに配置（Project scope相当）
mkdir -p .claude/skills
cp -r skills/skills/skill-creator .claude/skills/
```

> You can also manually install skills by adding them to ~/.claude/skills.
> 
> — [Introducing Agent Skills](https://www.anthropic.com/news/skills)

**手動配置時のパス対応：**

| 手動配置先 | 相当するスコープ | 備考 |
| --- | --- | --- |
| `~/.claude/skills/` | User scope | すべてのプロジェクトで使用可能 |
| `.claude/skills/` | Project scope | Git管理に含めればチーム共有可能 |

#### 方法3：ローカルディレクトリから追加

```
/plugin add /path/to/skill-directory
```

### 5.2 /pluginコマンドのUI操作

インストール後、 `/plugin` コマンドを実行すると以下のような対話的UIが表示されます。

```
╭─────────────────────────────────────────────────────────────────────────────╮
│  Discover   Installed   Marketplaces   Errors  (tab to cycle)               │
│                                                                             │
│ Discover plugins (1/41)                                                     │
│ ╭─────────────────────────────────────────────────────────────────────────╮ │
│ │ ⌕ Search…                                                               │ │
│ ╰─────────────────────────────────────────────────────────────────────────╯ │
│                                                                             │
│ ❯ ◯ context7 · claude-plugins-official · 36.2K installs                     │
│     Upstash Context7 MCP server for up-to-date documentation ...            │
│                                                                             │
│   ◯ frontend-design · claude-plugins-official · 32.6K installs              │
│     Create distinctive, production-grade frontend interfaces ...            │
│                                                                             │
│   ◯ example-skills · anthropic-agent-skills · 5.2K installs                 │
│     Collection of example skills demonstrating various capabi...            │
│                                                                             │
╰─────────────────────────────────────────────────────────────────────────────╯
   Type to search · Space: (de)select · Enter: details · Esc: back
```

**4つのタブ：**

| タブ | 説明 |
| --- | --- |
| **Discover** | 登録済みマーケットプレイスから利用可能なプラグインを検索・インストール |
| **Installed** | インストール済みプラグインの一覧表示・有効/無効切り替え・アンインストール |
| **Marketplaces** | 登録済みマーケットプレイスの管理（追加・削除） |
| **Errors** | プラグイン関連のエラー表示 |

**操作方法：**

| キー | 動作 |
| --- | --- |
| `Tab` | タブを切り替え |
| `↑` `↓` | プラグインを選択 |
| `Space` | プラグインを選択/解除（複数選択可能） |
| `Enter` | 選択したプラグインの詳細表示・インストール画面へ |
| `Esc` | 前の画面に戻る |
| 文字入力 | プラグインを検索 |

**Installedタブでの管理：**

```
╭─────────────────────────────────────────────────────────────────────────────╮
│  Discover   Installed   Marketplaces   Errors                               │
│                                                                             │
│ Installed plugins                                                           │
│                                                                             │
│ User scope:                                                                 │
│   ✓ example-skills (anthropic-agent-skills)                                 │
│   ✓ document-skills (anthropic-agent-skills)                                │
│                                                                             │
│ Project scope:                                                              │
│   ✓ custom-skill (local)                                                    │
│                                                                             │
╰─────────────────────────────────────────────────────────────────────────────╯
```

Installedタブでは、プラグインがスコープ別にグループ化されて表示されます。ここから有効/無効の切り替えやアンインストールが可能です。

### 5.3 インストール後の使い方

**重要：インストール後は「言及するだけ」でSkillが発動します。**

> After installing the plugin, you can use the skill by just mentioning it.
> 
> — [anthropics/skills README](https://github.com/anthropics/skills)

#### 使用例

```
# PDFスキルを使う場合
「Use the PDF skill to extract the form fields from path/to/some-file.pdf」

# skill-creatorを使う場合
「新しいSkillを作りたい。skill-creatorを使って」

# algorithmic-artを使う場合
「algorithmic-artスキルでジェネレーティブアートを作って」

# mcp-builderを使う場合
「MCPサーバーを作りたい。mcp-builderを使って」
```

#### 実行フローの可視化

### 5.4 Claude.aiでの使用方法

Claude.ai（有料プラン）では、example-skillsは最初から利用可能です。

特別なインストール作業は不要で、Skillに関連するタスクを依頼するとClaudeが自動的に適切なSkillを参照します。

カスタムSkillをアップロードする場合：

1. **Settings > Capabilities** に移動
2. **Skills** セクションでSkillをアップロード
3. トグルでSkillの有効/無効を切り替え

参考： [Using Skills in Claude | Claude Help Center](https://support.claude.com/en/articles/12512180-using-skills-in-claude)

### 5.5 Skillの実例を参考にする

**自分のSkillを作る前に、既存のSkillを読むことを強くおすすめします。**

リポジトリ内の各Skillは、以下のような構成になっています。

```
algorithmic-art/
├── SKILL.md              # メイン指示
├── templates/
│   ├── viewer.html       # 表示用テンプレート
│   └── generator_template.js
└── README.md

mcp-builder/
├── SKILL.md
├── reference/            # API仕様等
└── scripts/              # ユーティリティ
```

これらを読むことで、Progressive Disclosureの実践例や、scripts/references/assetsの使い分けが理解できます。

---

## 6\. skill-creatorを使ったSkill作成：ステップバイステップ

### Step 1：対話による要件定義

skill-creatorに「○○用のSkillを作りたい」と伝えると、Claudeが以下のような質問をしてきます。

**Claudeからの質問例：**

- 「このSkillはどのような機能をサポートすべきですか？」
- 「具体的な使用例を教えてください」
- 「どのようなトリガーでこのSkillが発動すべきですか？」

**回答のコツ：**

- 具体的なユースケースを3つ以上挙げる
- 「○○と言われたら発動する」というトリガー条件を明確にする
- 既存の類似Skill（あれば）との違いを説明する

### Step 2：init\_skill.pyによる初期化

要件が固まると、Claudeが以下のコマンドを実行します。

```
python init_skill.py my-new-skill --path skills/public
```

**生成されるファイル：**

```
my-new-skill/
├── SKILL.md              # メインの指示ファイル
├── scripts/
│   └── example.py        # サンプルスクリプト
├── references/
│   └── example.md        # サンプル参照ドキュメント
└── assets/
    └── example.txt       # サンプルアセット
```

### Step 3：SKILL.md・リソースのカスタマイズ

生成されたテンプレートをカスタマイズします。

**SKILL.mdのフロントマター（必須フィールド）：**

```
---
name: my-new-skill
description: "このSkillの説明。Claudeがいつこのskillを使うべきか判断する材料になる。200文字以内で記述。"
---
```

**オプションフィールド：**

```
---
name: my-new-skill
description: "説明文"
license: MIT
allowed-tools:
  - bash
  - python
metadata:
  author: your-name
  version: "1.0.0"
---
```

> **注意：** `version` や `author` をトップレベルに書くとエラーになります。これらは `metadata` の下に記述してください。
> 
> 参考： [Issue #37: Generated skills fail validation](https://github.com/anthropics/skills/issues/37)

**Markdown本文の書き方：**

```
# My New Skill

## Overview
このSkillは○○を実現するためのものです。

## When to Use
- ○○と言われたとき
- ○○のファイルを処理するとき

## How to Use
1. まず○○を確認
2. 次に○○を実行
3. 最後に○○を検証

## Examples
- 入力例：「○○してください」
- 出力例：○○が完了しました
```

### Step 4：検証とパッケージング

#### バリデーション

```
python quick_validate.py my-new-skill/
```

**チェック項目：**

- SKILL.mdの存在
- YAMLフロントマターの形式
- 必須フィールド（name, description）の存在
- 許可されていないプロパティの検出
- description文字数（1024文字以内）

#### パッケージング

```
python package_skill.py my-new-skill/ ./dist
```

これにより `my-new-skill.skill` ファイルが生成されます。

### ワークフロー全体図

---

## 7\. Fit & Gap分析：skill-creatorの適用判断

skill-creatorは万能ではありません。適切な場面で使用することが重要です。

### 7.1 Fit（適している場面）

| 場面 | 理由 |
| --- | --- |
| 繰り返し発生するタスク | Skillとしてパッケージ化する価値がある |
| 手順が明確なワークフロー | SKILL.mdに記述しやすい |
| コード実行を伴うタスク | scripts/ディレクトリが活用できる |
| チームで共有したいナレッジ | .skillファイルで配布可能 |
| 決まったフォーマットの出力 | assets/にテンプレートを配置できる |

### 7.2 Gap（適さない場面）

| 場面 | 理由 | 代替案 |
| --- | --- | --- |
| 一度きりのタスク | Skill化のコストに見合わない | 通常のプロンプトで対応 |
| 頻繁に変わる要件 | メンテナンスコストが高い | Memory機能を活用 |
| 外部APIとの連携が主 | MCPサーバーの方が適切 | MCPを検討 |
| 機密情報を含むタスク | Skillに機密情報を含めるリスク | セキュアな方法を検討 |

### 7.3 判断フローチャート

---

## 8\. 注意点とベストプラクティス

### 8.1 セキュリティ上の注意

Skillsはコード実行を伴うため、セキュリティには十分な注意が必要です。

> We strongly recommend using Skills only from trusted sources: those you created yourself or obtained from Anthropic.
> 
> — [Agent Skills - Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)

**チェックリスト：**

- APIキーやパスワードをハードコードしていないか
- 外部ネットワークへの意図しない接続がないか
- ファイルアクセスの範囲は適切か
- 第三者のSkillを使う場合、コードを監査したか

### 8.2 効果的なSkill設計のコツ

**1\. descriptionを明確に書く**

Claudeは `description` を見てSkillを発動するかどうかを判断します。

```
# Bad
description: "PDF処理"

# Good
description: "PDFからテキスト・表を抽出し、フォームに入力し、ドキュメントをマージする"
```

**2\. コンテキストウィンドウを意識する**

> The context window is a public good. Skills share the context window with everything else Claude needs.
> 
> — [skill-creator SKILL.md](https://github.com/anthropics/skills/blob/main/skill-creator/SKILL.md)

- Claudeが既に知っていることは書かない
- 冗長な説明より簡潔な例を優先
- 相互排他的な情報は別ファイルに分離

**3\. 命令形で書く**

Skillの指示は二人称（You should...）ではなく、命令形（To do X, do Y）で書きます。

```
# Bad
You should first check the file format.

# Good
To process the file, first verify the format using the validate.py script.
```

**4\. 段階的に改善する**

一度で完璧なSkillを作ろうとせず、実際に使いながら改善します。

1. Skillを使って実タスクを実行
2. 問題点や非効率な点を特定
3. SKILL.mdやリソースを更新
4. 再度テスト

---

## 9\. まとめ

skill-creatorは、Skill作成の敷居を大幅に下げるツールです。

**ポイント：**

- 自然言語で対話しながらSkillを設計できる
- init\_skill.py / quick\_validate.py / package\_skill.py の3スクリプトで作成フローを自動化
- Progressive Disclosure（段階的開示）の原則を理解することが重要
- セキュリティには十分注意し、信頼できるソースのSkillのみを使用する

Skillを活用することで、チームのナレッジを再利用可能な形で蓄積し、Claudeをより効果的に活用できるようになります。

---

## 参考リンク

- [anthropics/skills - GitHub](https://github.com/anthropics/skills)
- [Introducing Agent Skills | Anthropic](https://www.anthropic.com/news/skills)
- [Equipping agents for the real world with Agent Skills | Anthropic Engineering](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- [How to create custom Skills | Claude Help Center](https://support.claude.com/en/articles/12512198-how-to-create-custom-skills)
- [Using Skills in Claude | Claude Help Center](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [Agent Skills - Claude Docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)

---

## 付録：日本語版スキルリポジトリ（skills-ja）の使い方

本リポジトリ（ [skills-ja](https://github.com/yamato-snow/skills) ）は、Anthropic公式の [anthropics/skills](https://github.com/anthropics/skills) をフォークし、 **全スキルを日本語に翻訳** したものです。

### 10.1 skills-jaの特徴

| 項目 | 公式リポジトリ | skills-ja |
| --- | --- | --- |
| 言語 | 英語 | 日本語 |
| SKILL.mdのdescription | 英語 | 日本語 |
| スキル本文 | 英語 | 日本語 |
| コード・コマンド | そのまま | そのまま（変更なし） |
| 追加スキル | なし | ui-ux-pro-max（外部から追加） |

### 10.2 インストール方法

#### 方法A：マーケットプレイスに登録してインストール（推奨）

```
# Step 1: skills-jaをマーケットプレイスとして登録
/plugin marketplace add yamato-snow/skills

# Step 2: プラグインをインストール
# ※マーケットプレイス名は「anthropic-agent-skills-ja」になります
/plugin install example-skills@anthropic-agent-skills-ja
/plugin install document-skills@anthropic-agent-skills-ja
```

#### 既存のマーケットプレイス・プラグインの確認と削除

公式版と競合する場合は、先に確認・削除してからインストールしてください。

```
# 登録済みマーケットプレイスを確認
/plugin
# → Marketplacesタブで確認

# マーケットプレイスを削除（UIから）
/plugin
# → Marketplacesタブ → 削除したいマーケットプレイスを選択 → Enter → Remove

# インストール済みプラグインを確認
/plugin
# → Installedタブで確認

# プラグインをアンインストール（UIから）
/plugin
# → Installedタブ → 削除したいプラグインを選択 → Enter → Uninstall
```

**公式版から日本語版に切り替える場合の手順：**

1. `/plugin` → Installedタブ → `example-skills@anthropic-agent-skills` を選択 → Uninstall
2. `/plugin` → Installedタブ → `document-skills@anthropic-agent-skills` を選択 → Uninstall
3. `/plugin marketplace add yamato-snow/skills` で日本語版を登録
4. `/plugin install example-skills@anthropic-agent-skills-ja` でインストール
5. `/plugin install document-skills@anthropic-agent-skills-ja` でインストール
6. Claude Codeを再起動して変更を適用

#### 方法B：ローカルにcloneして手動配置

```
# リポジトリをclone
git clone https://github.com/yamato-snow/skills.git skills-ja

# User scopeとして配置（全プロジェクトで使用可能）
mkdir -p ~/.claude/skills
cp -r skills-ja/skills/* ~/.claude/skills/

# または Project scopeとして配置（このプロジェクトのみ）
mkdir -p .claude/skills
cp -r skills-ja/skills/* .claude/skills/
```

### 10.3 スキルの使い方（発動方法）

スキルを発動させるには、以下の3つの方法があります。

#### 方法1：自然言語で言及する（最も簡単）

スキル名やその機能を会話の中で言及するだけで、Claudeが自動的にスキルを発動します。

```
# 例：PDFスキルを使いたい場合
「このPDFからテキストを抽出して」
「PDFスキルでフォームフィールドを取得して」

# 例：docxスキルを使いたい場合
「Word文書を作成して」
「docxスキルで報告書を作って」

# 例：pptxスキルを使いたい場合
「プレゼンテーションを作成して」
「PowerPointでスライドを作って」

# 例：xlsxスキルを使いたい場合
「Excelファイルを作成して」
「スプレッドシートにデータを入力して」
```

#### 方法2：スキル名を明示的に指定する

より確実にスキルを発動させたい場合は、スキル名を明示的に指定します。

```
「skill-creatorを使って新しいスキルを作りたい」
「mcp-builderでMCPサーバーを構築して」
「algorithmic-artスキルでジェネレーティブアートを生成して」
「frontend-designスキルでランディングページを作って」
```

#### 方法3：スラッシュコマンドを使う（一部のスキル）

一部のスキルはスラッシュコマンドで直接呼び出せます。

```
/skill-creator
/pdf
/docx
```

### 10.4 スキル選択のUIについて

Claude Codeでは、 `/plugin` コマンドでインストール済みスキルを確認・管理できます。

```
/plugin
```

表示されるUIでの操作：

| キー | 動作 |
| --- | --- |
| `Tab` | Discover / Installed / Marketplaces / Errors を切り替え |
| `↑` `↓` | スキル・プラグインを選択 |
| `Space` | 選択/解除（複数選択可） |
| `Enter` | 詳細表示・操作メニューへ |
| `Esc` | 戻る |

**Installedタブ** で、インストール済みスキルの有効/無効を切り替えられます。

### 10.5 日本語スキル一覧

skills-jaには以下の日本語化されたスキルが含まれています：

| カテゴリ | スキル名 | 用途 |
| --- | --- | --- |
| **ドキュメント** | pdf | PDFの読み取り・作成・結合・分割・フォーム入力 |
|  | docx | Word文書の作成・編集・変更履歴操作 |
|  | pptx | PowerPointプレゼンテーションの作成・編集 |
|  | xlsx | Excelスプレッドシートの作成・編集・数式処理 |
| **クリエイティブ** | algorithmic-art | p5.jsでアルゴリズムアート生成 |
|  | canvas-design | PNG/PDF形式のビジュアルデザイン作成 |
|  | slack-gif-creator | Slack用アニメーションGIF作成 |
| **開発** | mcp-builder | MCPサーバー構築ガイド |
|  | webapp-testing | Playwrightでローカルアプリテスト |
|  | web-artifacts-builder | React/Tailwind/shadcn-uiでArtifact構築 |
|  | frontend-design | 高品質なフロントエンドUI作成 |
| **エンタープライズ** | brand-guidelines | Anthropicブランドガイドライン適用 |
|  | internal-comms | 社内コミュニケーション文書作成 |
|  | theme-factory | 10種類のプリセットテーマ適用 |
|  | doc-coauthoring | ドキュメント共同執筆ワークフロー |
| **メタ** | skill-creator | Skill作成支援 |
| **追加** | ui-ux-pro-max | プロフェッショナルなUI/UXデザイン支援 ※ |

※ **ui-ux-pro-max** は [nextlevelbuilder/ui-ux-pro-max-skill](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) からの追加スキルです。

### 10.6 使用例

#### 例1：日本語でPDFを操作

```
ユーザー：「請求書.pdfからテキストを抽出して、金額を合計して」

Claude：PDFスキルを使用してテキストを抽出します...
（SKILL.mdの日本語指示に従って処理を実行）
```

#### 例2：日本語でプレゼン作成

```
ユーザー：「来週の会議用にプレゼンを作って。テーマは新製品発表」

Claude：pptxスキルを使用してプレゼンテーションを作成します...
（日本語化されたSKILL.mdに従い、適切なレイアウトで作成）
```

#### 例3：新しいスキルを作成

```
ユーザー：「社内のコードレビュー用スキルを作りたい」

Claude：skill-creatorを使用してスキルを設計します。
いくつか質問させてください...
- どのような機能をサポートすべきですか？
- 具体的な使用例を教えてください
- どのようなトリガーで発動すべきですか？
```

### 10.7 トラブルシューティング

| 問題 | 解決策 |
| --- | --- |
| スキルが発動しない | `/plugin` でInstalledタブを確認し、スキルが有効になっているか確認 |
| 日本語が文字化けする | ファイルがUTF-8で保存されているか確認 |
| 特定のスキルだけ使いたい | スキル名を明示的に指定して呼び出す |
| 公式版と併用したい | 別のスコープ（User/Project/Local）に分けてインストール |

---

## 11\. 更新履歴

| 日付 | 内容 |
| --- | --- |
| 2025-12-25 | skills-jaリポジトリの使用方法セクションを追加 |
| 2025-12-24 | 初版公開 |

136

84