---
title: "【Claude】Agent Skills入門 - はじめてのスキル作成 -"
source: "https://tech.findy.co.jp/entry/2025/10/27/070000"
author:
  - "[[starfish719]]"
  - "[[ひらき (id:h-piiice16)]]"
  - "[[たかぼー (id:Taka_bow)]]"
  - "[[Shun (id:ShunDeveloper)]]"
  - "[[kouzyun]]"
  - "[[rokumura7]]"
  - "[[chida (id:t-chida0909)]]"
  - "[[たがしらけいすけ (id:tagasyksk)]]"
  - "[[shoota-dev]]"
  - "[[ham (id:hamchance0215)]]"
published: 2025-10-27
created: 2026-01-05
description: "こんにちは。 ファインディ株式会社でテックリードマネージャーをやらせてもらっている戸田です。 現在のソフトウェア開発の世界は、生成AIの登場により大きな転換点を迎えています。 GitHub Copilot や Claude Code など、生成AIを活用した開発支援ツールが次々と登場し、日常的なワークフローに組み込まれつつあります。 そんな中で先日、Claudeの新機能であるAgent Skillsが公開されました。 そこで今回は、Agent Skillsの紹介と解説、スキルの作り方を紹介したいと思います。 それでは見ていきましょう！ Agent Skillsとは 作り方 ファイル構成 ski…"
tags:
  - "clippings"
image: "https://cdn.image.st-hatena.com/image/scale/5b55661a33e07c4eef6aa27c284d530e3ad817ff/backend=imagemagick;version=1;width=1300/https%3A%2F%2Fcdn-ak.f.st-hatena.com%2Fimages%2Ffotolife%2Fs%2Fstarfish719%2F20251024%2F20251024111049.png"
---
こんにちは。

ファインディ株式会社でテックリードマネージャーをやらせてもらっている戸田です。

現在のソフトウェア開発の世界は、生成AIの登場により大きな転換点を迎えています。

GitHub Copilot や Claude Code など、生成AIを活用した開発支援ツールが次々と登場し、日常的なワークフローに組み込まれつつあります。

そんな中で先日、Claudeの新機能であるAgent Skillsが公開されました。

そこで今回は、Agent Skillsの紹介と解説、スキルの作り方を紹介したいと思います。

それでは見ていきましょう！

## Agent Skillsとは

Agent SkillsはClaudeの機能を用途や状況に応じて柔軟に拡張できる便利な機能となっています。

[docs.claude.com](https://docs.claude.com/en/docs/claude-code/skills)

Claude Code のバージョンが1.0以上であれば、誰でも簡単に利用することが可能です。

スキルはClaudeが必要に応じて読み込むための指示を含む `SKILL.md` ファイルと、スクリプトやテンプレートなどのオプションのサポートファイルで構成されています。

スキルはモデルによって呼び出されます。Claudeはユーザーが入力したプロンプトとスキルの名前、説明に基づいて、いつどのスキルを実行するかを自律的に決定します。

公式ドキュメントによると、主なメリットは次の３つが挙げられています。

- Claude の特化：ドメイン固有のタスクに合わせて機能をカスタマイズ。
- 繰り返し作業の削減：一度作成すれば自動的に使用可能。プロンプトの属人化を防ぎ、コード化したワークフローとしてClaudeに機能拡張する。
- 機能の合成：スキルを組み合わせて複雑なワークフローを構築。

スキルは再利用可能なファイルシステムベースのリソースであり、Claudeにドメイン固有の専門知識（ワークフロー、コンテキスト、そして汎用エージェントをスペシャリストへと進化させるベストプラクティス）を提供します。

プロンプトとは異なりスキルはオンデマンドで読み込まれるため、複数の会話で同じガイダンスを繰り返し提供する必要がなくなります。

ここまで読むとSubAgentやシステムプロンプト、MCPと何が違うのか良くわからない読者の方もいると思います。

Agent Skillsの特徴として **Progressive disclosure** が挙げられます。これは必要な情報のみが段階的に開示されることを意味しています。この仕組みにより、Claudeは幾つものスキルを同時に認識しながらも、コンテキストが肥大化することを防いでいます。

システムプロンプトやMCPとの決定的な違いはここにあります。両者は事前にClaudeがシステムプロンプトの内容やMCPが提供する全てのtoolの説明文と引数などを把握する必要があるため、必要以上にコンテキストが肥大化してしまう傾向があります。

一方、Agent Skillsはファイルシステムベースで動くだけでなく、最初は必要最低限の情報のみ把握します。そして該当するスキルを実行するときに、そのスキルの詳細のみを読み込みにいくのでコンテキストが必要以上に肥大化することを防いでいます。

## 作り方

### ファイル構成

基本的には `.claude/skills` の配下にスキル用のフォルダと `SKILL.md` ファイルを作成するだけでスキルを作成できます。

`SKILL.md` の中身は次のような内容になります。YAMLとマークダウンで記述されます。

```markdown
---
name: Your Skill Name
description: Brief description of what this Skill does and when to use it
---

# Your Skill Name

## Instructions
Provide clear, step-by-step guidance for Claude.

## Examples
Show concrete examples of using this Skill.
```

まずYAMLの部分の解説をします。

```markdown
---
name: Your Skill Name
description: Brief description of what this Skill does and when to use it
---
```

これはメタデータと呼ばれ、スキルを作成するうえで非常に重要な要素です。

Claudeは起動時にメタデータを読み込み、各スキルの存在と使用タイミングのみを認識してシステムプロンプトに組み込みます。このアプローチにより必要以上にコンテキストを肥大化することなく、多くのスキルを用意できます。

スキルのメタデータに一致するプロンプト実行やリクエストがあると、Claudeはファイルシステムから `SKILL.md` を読み取ります。

実際に実行されるかどうかの精度はメタデータの内容によって大きく左右されるので、非常に重要な要素となっています。

次にコンテンツ部分の解説をします。

```markdown
# Your Skill Name

## Instructions
Provide clear, step-by-step guidance for Claude.

## Examples
Show concrete examples of using this Skill.
```

メタデータはClaudeの起動時に必ず読み込まれますが、コンテンツ部分は実行時に読み込まれます。そして、エージェントスキルの実行時にコンテンツ部分に記載された内容を元にClaudeが処理を実行します。

ここで重要なのは、Agent Skillsが効率的な読み込みを実行していたとしても、コンテンツ部分の内容も簡潔にした方が良いという点です。Claudeがコンテンツ部分を読み込むと、その内容が会話履歴および他のコンテキストと競合します。

そのため、 `CLAUDE.md` やシステムプロンプトに記述されている内容やプログラム言語、ライブラリなどの一般的な内容についての言及はコンテンツ部分では省略して記述しましょう。どの部分を省略して、どこからをコンテンツ部分に記述するかの見極めをすることが、精度の高いスキルを作成するコツの1つです。

### skill-creator

スキルを簡単に作成するための仕組みとして、Anthropics社のリポジトリに `skill-creator` というプラグインが用意されています。今回はこちらを使って作成してみましょう。

[github.com](https://github.com/anthropics/skills)

Claude Codeを立ち上げて `/plugin` コマンドを実行します。

```bash
> /plugin 
╭─────────────────────────────────────────────────────────────────────╮
│ Plugins                                                             │
│                                                                     │
│ ❯ 1. Browse and install plugins                                     │
│   2. Manage and uninstall plugins                                   │
│   3. Add marketplace                                                │
│   4. Manage marketplaces                                            │
│   5. View installation status (errors)                              │
╰─────────────────────────────────────────────────────────────────────╯
```

`4. Manage marketplaces` を選択しましょう。

```bash
> /plugin 
╭─────────────────────────────────────────────────────────────────────╮
│ No marketplaces configured.                                         │
╰─────────────────────────────────────────────────────────────────────╯
```

マーケットプレイスの登録が無いようです。 `anthropics/skills` をマーケットプレイスとして登録します。

```bash
> /plugin marketplace add anthropics/skills 
  ⎿  Successfully added marketplace: anthropic-agent-skills
```

登録したマーケットプレイスから `example-skills` というプラグインをインストールしましょう。

```bash
> /plugin install example-skills@anthropic-agent-skills 
  ⎿ ✓ Installed example-skills. Restart Claude Code to load new 
    plugins.
```

再度 `/plugin` コマンドから `4. Manage marketplaces` を選択しましょう。今度はマーケットプレイスが登録されていることがわかります。 `anthropic-agent-skills` を選択しましょう。

```bash
> /plugin 
╭─────────────────────────────────────────────────────────────────────╮
│ Manage marketplaces                                                 │
│                                                                     │
│ ❯ ● anthropic-agent-skills                                          │
│     anthropics/skills                                               │
│     2 available • 1 installed • Updated 10/20/2025                  │
│                                                                     │
╰─────────────────────────────────────────────────────────────────────╯
```

`example-skills` がinstallされていることを確認できました。

```bash
> /plugin 
╭─────────────────────────────────────────────────────────────────────╮
│ anthropic-agent-skills                                              │
│ anthropics/skills                                                   │
│ Last updated: 10/20/2025                                            │
│                                                                     │
│ 2 available plugins                                                 │
│                                                                     │
│ Installed plugins (1):                                              │
│  ● example-skills                                                   │
│    Collection of example skills demonstrating various capabilities  │
│    including skill creation, MCP building, visual design,           │
│    algorithmic art, internal communications, web testing, artifact  │
│    building, Slack GIFs, and theme styling                          │
│                                                                     │
│ ❯ Update marketplace                                                │
│   Remove marketplace                                                │
╰─────────────────────────────────────────────────────────────────────╯
```

これで `skill-creator` を実行できるようになってるはずです。

## 実践編

では実際に `skill-creator` を使ってスキルを作ってみましょう。

今回はコミットメッセージのルールをスキルとして作成する例を紹介します。

弊社ではAPIのリリースをSemantic Versioningで管理しています。そこで重要なのがコミットメッセージです。コミットメッセージにSemantic Versioningを入れるルールとなっており、リリース時にコミットログからリリースバージョンを自動的に計算するようにしています。

まずは作成したいスキルの概要と `skill-creator` を使って作成する旨をプロンプトで入力します。

```bash
▐▛███▜▌   Claude Code v2.0.25
▝▜█████▛▘  Sonnet 4.5 · API Usage Billing
  ▘▘ ▝▝    /Users/hoge/project/github/Repo

───────────────────────────────────────────────────────────────────────
> コミットメッセージのルールを提供するスキルをskill-creatorを使って
  作成してください。
  semantic versioningをコミットメッセージの先頭に付与してください。 
───────────────────────────────────────────────────────────────────────
```

すると先ほどインストールした `example-skills:skill-creator` を使う旨のレスポンスが返ってきます。Yesを選択して作成してもらいましょう。

```bash
⏺ コミットメッセージのルールを提供するスキルを作成します。skill-cre
  atorを使用して、semantic versioningをコミットメッセージの先頭に付与
  するスキルを作成しましょう。

╭─────────────────────────────────────────────────────────────────────╮
│ Use skill “example-skills:skill-creator”?                           │
│ Claude may use instructions, code, or files from this Skill.        │
│                                                                     │
│   Guide for creating effective skills. This skill should be used    │
│   when users want to create a new skill (or update an existing      │
│   skill) that extends Claude's capabilities with specialized        │
│   knowledge, workflows, or tool integrations.                       │
│   (plugin:example-skills@anthropic-agent-skills)                    │
│                                                                     │
│ Do you want to proceed?                                             │
│ ❯ 1. Yes                                                            │
│   2. Yes, and don't ask again for example-skills:skill-creator in   │
│   /Users/hoge/project/github/Repo                           │
│   3. No, and tell Claude what to do differently (esc)               │
│                                                                     │
╰─────────────────────────────────────────────────────────────────────╯
```

ここで初めて `skill-creator` のスキルのコンテンツ部分が読み込まれます。

Claudeが具体的な内容の認識を合わせようとしてくれています。ここで認識が違う場合はVibe Codingの要領で変更をしましょう。今回は特に問題ないのでOKと入力して続けます。

```bash
> The "skill-creator" skill is loading 

> The "skill-creator" skill is loading 

⏺ skill-creatorスキルが読み込まれました。では、semantic versioningをコ
  ミットメッセージの先頭に付与するスキルを作成していきます。

  まず、具体的な使用例を確認させてください。
  どのようなコミットメッセージルールを想定されていますか?

  例えば：
  - major-feat: 新しい機能を追加 - 破壊的変更を含む新機能
  - minor-feat: 既存機能の拡張 - 後方互換性のある機能追加
  - patch-fix: バグ修正 - 後方互換性のあるバグ修正
  - patch-refactor: コードのリファクタリング - 動作を変えない改善

  このようなフォーマットで良いでしょうか？また、以下の点も確認させ
  てください：

  1. Semantic Versioningの適用方法: {version-type}-{change-type}: 
  {description} という形式でよろしいですか？
  2. 変更タイプ: feat, fix, refactor, docs, test
  などのタイプを使用しますか？
  3. 自動判定: コミット内容から自動的にversion-typeを判定する必要が
  ありますか？それとも開発者に確認しますか？
```

`skill-creator` が実行されて、エージェントスキルのテンプレートファイルが作成されます。

```bash
⏺ ありがとうございます。それでは、semantic
  versionをコミットメッセージの先頭に付与するスキルを作成します。

⏺ Bash(python /Users/hoge/.claude/plugins/marketplaces/anthropi
      c-agent-skills/skill-creator/scripts/init_skill.py commit-message
       --path .claude/skills)
  ⎿  🚀 Initializing skill: commit-message
        Location: .claude/skills
     … +16 lines (ctrl+o to expand)
```
```markdown
.claude/skills/commit-message/
├── SKILL.md
├── assets/
│   └── example_asset.txt
│── references/
│   └── api_references.md
└── scripts/
    └── example.py
```

続けて `SKILL.md` にエージェントスキルの内容を記述していきます。

```bash
⏺ Read(.claude/skills/commit-message/SKILL.md)
  ⎿  Read 86 lines

⏺ 次に、SKILL.mdを作成します。semantic versioningルールに基づいたコ
  ミットメッセージを生成するスキルを書いていきます。

⏺ Write(.claude/skills/commit-message/SKILL.md)

───────────────────────────────────────────────────────────────────────
 Opened changes in Visual Studio Code ⧉

 Save file to continue…

 Do you want to make this edit to SKILL.md?
 ❯ 1. Yes
   2. Yes, allow all edits during this session (shift+tab)
   3. No, and tell Claude what to do differently (esc)
```

次に参照ファイルを作成します。 `SKILL.md` からリファレンスファイルとして参照してもらうファイルが必要になるケースもあります。

```bash
⏺ 次に、referencesディレクトリにsemantic
  versioningの詳細なリファレンスを追加します。

⏺ Write(.claude/skills/commit-message/references/semantic_versioning.md
       )
───────────────────────────────────────────────────────────────────────
 Opened changes in Visual Studio Code ⧉

 Save file to continue…

 Do you want to make this edit to semantic_versioning.md?
 ❯ 1. Yes
   2. Yes, allow all edits during this session (shift+tab)
   3. No, and tell Claude what to do differently (esc)
```

結果的にテンプレートで使わなかったファイルを自動で削除してくれます。

```bash
⏺ 次に、不要なサンプルファイルを削除します。

⏺ Bash(rm -rf .claude/skills/commit-message/scripts
      .claude/skills/commit-message/assets
      .claude/skills/commit-message/references/api_reference.md)
  ⎿  (No content)
```

最後にzipでパッケージングしてくれます。Claude.aiで作成したスキルを使いたい時に、このzipファイルをアップロードすることで利用可能になるようです。

```bash
⏺ それでは、スキルをパッケージングしましょう。

⏺ Bash(python /Users/hoge/.claude/plugins/marketplaces/anthropi
      c-agent-skills/skill-creator/scripts/package_skill.py
      .claude/skills/commit-message)
  ⎿  📦 Packaging skill: .claude/skills/commit-message

     🔍 Validating skill...
     … +7 lines (ctrl+o to expand)
```

これでスキルの作成が完了しました。

```bash
⏺ 完了しました！semantic
  versionをコミットメッセージの先頭に付与するスキルを作成しました。

  作成したスキルの内容

  📁 スキル構成

  .claude/skills/commit-message/
  ├── SKILL.md                              # メインスキルファイル
  └── references/
      └── semantic_versioning.md            # 詳細なリファレンス

  🎯 主な機能

  1. コミットメッセージフォーマット
  {version-type}-{change-type}: {description}

  🤖 Generated with [Claude Code](https://claude.com/claude-code)

  Co-Authored-By: Claude <noreply@anthropic.com>
  2. Version Type（バージョンタイプ）
    - major: 破壊的変更（APIの変更、削除など）
    - minor: 新機能追加（後方互換性あり）
    - patch: バグ修正・小さな改善
  3. Change Type（変更タイプ）
    - feat: 新機能
    - fix: バグ修正
    - refactor: リファクタリング
    - perf: パフォーマンス改善
    - docs: ドキュメント
    - test: テスト
    - chore: メンテナンス
    - style: コードスタイル
  4. 決定フロー
    - 破壊的変更があるか？ → major
    - 新機能を追加するか？ → minor
    - それ以外 → patch
  5. 豊富な例とリファレンス
    - 具体的な使用例（4パターン）
    - 曖昧なシナリオの判断基準
    - エッジケースの扱い方

  📦 パッケージファイル

  commit-message.zip がプロジェクトルートに作成されました。このファ
  イルを他のプロジェクトで使用することもできます。

  🚀 使い方

  このスキルは自動的に有効になり、gitコミットを作成する際にClaude
  Codeが自動的に使用します。例：

  # ユーザー: "変更をコミットして"
  # → Claude Codeが自動的にcommit-messageスキルを使用
  # → semantic versioningに従ったコミットメッセージを生成

  スキルの準備が整いました！
```

では実際に作成したスキルを使ってみましょう。Claude Codeを再起動してコミットします。

```bash
> commitしてください 

⏺ コミットを作成します。まず、現在の変更内容を確認します。

⏺ Bash(git status)
  ⎿  On branch test-branch                       
     Changes to be committed:
       (use "git restore --staged <file>..." to unstage)
     … +8 lines (ctrl+o to expand)

⏺ Bash(git diff --staged)
  ⎿ diff --git a/.claude/skills/commit-message/SKILL.md b/.claude/
    skills/commit-message/SKILL.md
    index 4f094c4..49b2adf 100644
    … +702 lines (ctrl+o to expand)

⏺ Bash(git diff .claude/skills/commit-message/SKILL.md)
  ⎿  (No content)

⏺ Bash(git log -5 --oneline)
  ⎿ f384be4 Merge pull request #111 from Findy/fix/issue-222
    eef554d patch-refactor: fix hoge
    … +7 lines (ctrl+o to expand)

⏺ 変更内容を確認しました。このコミットメッセージはcommit-messageスキルに基づいて作成します。

╭─────────────────────────────────────────────────────────────────────╮
│ Use skill “commit-message”?                                         │
│ Claude may use instructions, code, or files from this Skill.        │
│                                                                     │
│   Rules for the commit message to be written when exec git commit   │
│    (project)                                                        │
│                                                                     │
│ Do you want to proceed?                                             │
│ ❯ 1. Yes                                                            │
│   2. Yes, and don't ask again for commit-message in                 │
│   /Users/hoge/project/github/Repo                           │
│   3. No, and tell Claude what to do differently (esc)               │
│                                                                     │
╰─────────────────────────────────────────────────────────────────────╯

⏺ ドキュメントの改善なので、patch-docsでコミットします。

⏺ Bash(git commit -m "patch-docs: enhance commit-message skill with
      comprehensive guidelines and examples…)
  ⎿ [test-branch 53ae227] patch-docs: enhance commit-messag
    e skill with comprehensive guidelines and examples
     2 files changed, 560 insertions(+), 60 deletions(-)
    … +2 lines (ctrl+o to expand)

⏺ Bash(git status)
  ⎿ On branch test-branch                                  
    Untracked files:
      (use "git add <file>..." to include in what will be committe
    … +5 lines (ctrl+o to expand)

⏺ コミットが正常に作成されました!

  作成したコミット:
  - コミットハッシュ: 53ae227
  - メッセージ: patch-docs: enhance commit-message skill with 
  comprehensive guidelines and examples

  変更内容:
  - .claude/skills/commit-message/SKILL.md を新規追加
  - references/semantic_versioning.md を新規追加
  - 合計560行の追加、60行の削除

  なお、commit-message.zip という未追跡ファイルが残っていますが、こ
  れは意図的に除外されています。
```

スキルが起動して、変更内容からSemantic Versioningを付与したコミットメッセージを自動で作成してコミットしてることが分かるかと思います。

```bash
hoge@Macbook Repo % git log
commit 4cb77a4d4332384a9acb7e67591ac3d32d08df21 (HEAD -> test-branch)
Author: hoge <hoge@test.com>
Date:   Wed Oct 22 14:50:42 2025 +0900

    patch-docs: enhance commit-message skill with comprehensive guidelines and examples
    
    🤖 Generated with [Claude Code](https://claude.com/claude-code)
    
    Co-Authored-By: Claude <noreply@anthropic.com>
```

このようにスキルを作成することも利用することも非常に簡単でシンプルであることを理解できたかと思います。

## まとめ

Agent Skillsは機能拡張されていく予定となっているらしく、今後ますます目が離せない機能となっています。

[www.anthropic.com](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)

ぜひ皆さんもAgent Skillsを活用して快適な開発環境を作ってみてください。

そしてこの度、 **11月13日(木)に福岡で Findy AI Meetup の開催が決定** しました。

[findy-inc.connpass.com](https://findy-inc.connpass.com/event/369691/)

今回は **YAPC::Fukuoka の前日の開催** です。県外からの参加者のみなさん、前日入りしてこちらへの参加もぜひ検討ください。

そしてなんと！これまでの福岡での開催が大盛況だったため、 **11/17(月)に東京オフィスでのFindy AI Meetupの開催も決定** しました！

[https://findy-inc.connpass.com/event/373552/](https://findy-inc.connpass.com/event/373552/) [findy-inc.connpass.com](https://findy-inc.connpass.com/event/373552/)

こちらの参加もぜひ検討ください。

現在、ファインディでは一緒に働くメンバーを募集中です。

興味がある方はこちらから ↓  
[herp.careers](https://herp.careers/v1/findy/requisition-groups/14c4a661-5e48-40c5-99d0-ea657b8b4c04)

[【エンジニアの日常】エンジニア達の人生… »](https://tech.findy.co.jp/entry/2025/10/24/070000)