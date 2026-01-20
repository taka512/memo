---
title: "Claude Code スキル自作勢へ！Agent Skills Marketplace に6万超スキル集結"
source: "https://izanami.dev/post/93b709b5-06ae-48a0-8489-9f1970c20756"
author:
  - "[[コムテ]]"
published: 2026-01-17
created: 2026-01-20
description: "Claude Code のスキルを自作する前に Agent Skills Marketplace をチェックすべき。65,635 個のオープンソーススキルが GitHub から集約され、AI セマンティクス検索で「やりたいこと」から探せる。Anthropic 公式も紹介してる"
tags:
  - "clippings"
image: "https://izanami.dev/post/93b709b5-06ae-48a0-8489-9f1970c20756/opengraph-image-1umokf?5589478d64c13f8a"
---
5

要約

Claude Code のスキルを自作する前に Agent Skills Marketplace をチェックすべき。65,635 個のオープンソーススキルが GitHub から集約され、AI セマンティクス検索で「やりたいこと」から探せる。Anthropic 公式も紹介してる

意見はこのエリアに表示されます

![アイキャッチ画像](https://izanami.dev/_next/image?url=https%3A%2F%2Fapi.izanami.dev%2Fstorage%2Fv1%2Fobject%2Fpublic%2Fpictures%2Feyecatch%2Ffb108034-0e3a-41b4-9484-6744a07be5ce%2F45cfaa0e-53e2-403f-a8ed-d211d068320a.png&w=1920&q=75&dpl=dpl_8eJ7e67ie4xVrFhZJ51beewmsz7c)

Claude Code のスキル、自作する前にここをチェックしてほしい

X で 毎日 AI 情報を配信してる [コムテ](https://x.com/commte) です。Agentic AI / AI 駆動開発 / Claude Code などを中心に情報を配信しています

この記事は [Agent Skills Marketplace](https://skillsmp.com/) を実際に触ってみた感想と、なぜこれが Claude Code ユーザーにとって革命的なのかを解説する

### なぜ今 Skills Marketplace が注目されてるのか

車輪の再発明をやめられるから

Claude Code を使い始めると、まず直面するのが「スキルってどう作ればいいの？」という壁よね

あ、スキルっていうのは、「こういう仕事をするときは、こう考えて、こう進める」という知識と手順をフォルダ単位で定義する感じやね

公式ドキュメント読んで、SKILL.md の書き方覚えて、試行錯誤しながら自分用のスキルを作っていく。これ自体は楽しいんやけど、実は先人たちがすでに大量のスキルを作って公開してくれてるんよ

[Agent Skills Marketplace](https://skillsmp.com/) というサービスがあって、ここに **65,635 個以上** のスキルが集約されてる。全部 GitHub のオープンソースで、SKILL.md 標準に準拠してるから、見つけたらすぐに使える

見てみると、我々が自分で作ったスキルよりいいものが揃ってる

それじゃあ、使わせてもらったほうがいいよね？

サードパーティってどうなん？って人もいるけど、

[Anthropic 公式ブログ](https://www.claude.com/blog/building-skills-for-claude-code) でも紹介されてて、LangChain の deepagent-CLI にも統合されてるレベルのサービスなんよね

### スキル数の爆発的増加

サイトのトレンドグラフを見ると、2024 年 12 月末から急増してるのがわかる。1 ヶ月で倍以上に増えてて、これはマジでエコシステムが加速してる証拠

なぜこんなに増えてるかというと、Skills の仕組み自体がシンプルやからなんよね。Markdown ファイル 1 つ書くだけでスキルになる

MCP みたいにサーバー立てたりコード書いたりする必要がない。だから参入障壁がめっちゃ低い

### カテゴリ別の内訳を見ていこう

Skills Marketplace では 10 個のメインカテゴリに分かれてて、それぞれにサブカテゴリがある。全部見ていくとボリュームあるけど、どこに何があるか把握しておくと探しやすくなるから解説していく

#### Tools カテゴリ（22,506 スキル）

一番多いのが Tools カテゴリ。開発者が日常的に使うツール系のスキルが集まってる

| サブカテゴリ | スキル数 |
| --- | --- |
| Productivity & Integration | 13,241 |
| Automation Tools | 6,532 |
| Debugging | 4,338 |
| System Administration | 2,613 |
| IDE Plugins | 2,277 |
| CLI Tools | 552 |
| Domain & DNS Tools | 136 |

Productivity & Integration が圧倒的に多い。これは Notion、Slack、Google Workspace などの連携スキルがここに入ってるから。自動化系も 6,000 以上あって、定型作業を Claude に任せたい人には宝の山やね

Debugging スキルが 4,338 もあるのは意外やった。エラーメッセージの解析とか、ログの読み方とか、そういう知識がスキルとして共有されてるんよね

#### Development カテゴリ（19,370 スキル）

開発関連のスキルがここに集約されてる

| サブカテゴリ | スキル数 |
| --- | --- |
| CMS & Platforms | 7,181 |
| Architecture Patterns | 5,136 |
| Full Stack | 3,596 |
| Frontend | 3,233 |
| Scripting | 1,974 |
| Game Development | 1,947 |
| Mobile | 1,578 |
| Backend | 1,303 |
| Package & Distribution | 710 |
| E-commerce Development | 693 |
| Framework Internals | 579 |

CMS & Platforms が 7,000 超えてるのは、WordPress、Shopify、Strapi などの各 CMS に特化したスキルがあるから。Architecture Patterns も 5,000 以上あって、設計パターンの知識をスキルとして取り込めるのは便利

Frontend と Backend が分かれてるのもいい。React、Vue、Next.js などのフレームワーク別にスキルがあるから、自分のスタックに合わせて選べる

#### Data & AI カテゴリ（12,997 スキル）

AI 関連のスキルがここ。Claude Code 使ってる人なら特に気になるカテゴリよね

| サブカテゴリ | スキル数 |
| --- | --- |
| LLM & AI | 10,293 |
| Data Analysis | 1,752 |
| Data Engineering | 1,491 |
| Machine Learning | 1,122 |

LLM & AI が **10,000 超え** してるのはすごい。プロンプトエンジニアリングのベストプラクティスとか、各 LLM の特性を活かした使い方とか、そういう知識がスキルとして共有されてる

Data Analysis と Data Engineering も合わせて 3,000 以上あるから、データ処理のワークフローを強化したい人にはかなり使える

#### Business カテゴリ（11,543 スキル）

ビジネス系のスキルも結構充実してる

| サブカテゴリ | スキル数 |
| --- | --- |
| Project Management | 7,339 |
| Sales & Marketing | 4,929 |
| Finance & Investment | 2,564 |
| Health & Fitness | 2,018 |
| Business Apps | 557 |
| Real Estate & Legal | 504 |
| Payment | 119 |
| E-commerce | 85 |

Project Management が 7,000 以上あるのは、タスク管理やプロジェクト進行のベストプラクティスがスキルとして共有されてるから。Sales & Marketing も 5,000 近くあって、マーケティング施策のアイデア出しとかに使えそう

Health & Fitness が 2,000 あるのは意外やった。ビジネスカテゴリに入ってるのは、ウェルネス系のビジネス向けってことかな

#### DevOps カテゴリ（10,839 スキル）

インフラ・運用系のスキルがここ

| サブカテゴリ | スキル数 |
| --- | --- |
| CI/CD | 6,015 |
| Git Workflows | 4,781 |
| Cloud | 1,773 |
| Containers | 1,186 |
| Monitoring | 814 |

CI/CD が 6,000 以上あるのはありがたい。GitHub Actions、GitLab CI、CircleCI などの各サービス別にスキルがあるから、自分の環境に合わせて選べる

Git Workflows も 4,781 あって、ブランチ戦略とかコミットメッセージの書き方とか、チーム開発のベストプラクティスがスキルとして取り込める

#### Testing & Security カテゴリ（7,984 スキル）

テストとセキュリティ関連

| サブカテゴリ | スキル数 |
| --- | --- |
| Testing | 3,427 |
| Code Quality | 3,097 |
| Security | 1,727 |

Testing と Code Quality で 6,500 以上。単体テストの書き方とか、コードレビューのポイントとか、品質を上げるための知識がスキルとして共有されてる

Security が 1,727 あるのもいい。OWASP Top 10 とか、セキュアコーディングのベストプラクティスとか、セキュリティの知識は常に最新化が必要やから、コミュニティで共有されてるのはありがたい

#### Documentation カテゴリ（5,672 スキル）

ドキュメント作成関連

| サブカテゴリ | スキル数 |
| --- | --- |
| Knowledge Base | 4,376 |
| Technical Docs | 1,733 |
| Education | 1,650 |

Knowledge Base が 4,000 以上あるのは、社内 Wiki とか FAQ の作成スキルがここに入ってるから。Technical Docs も 1,733 あって、API ドキュメントとか README の書き方のベストプラクティスがスキルとして使える

#### Content & Media カテゴリ（5,591 スキル）

コンテンツ制作関連

| サブカテゴリ | スキル数 |
| --- | --- |
| Content Creation | 2,925 |
| Documents | 1,935 |
| Design | 1,297 |
| Media | 463 |

Content Creation が 2,925 あるから、ブログ記事とか SNS 投稿の作成に使えるスキルが豊富。Design も 1,297 あって、デザインシステムとかブランドガイドラインのスキルがある

#### Research カテゴリ（2,653 スキル）

研究・学術関連

| サブカテゴリ | スキル数 |
| --- | --- |
| Academic | 1,543 |
| Scientific Computing | 640 |
| Computational Chemistry | 601 |
| Bioinformatics | 496 |
| Lab Tools | 109 |
| Astronomy & Physics | 97 |

研究者向けのスキルもあるんよね。Academic が 1,543 あって、論文の書き方とか文献レビューのベストプラクティスがスキルとして共有されてる

Bioinformatics とか Computational Chemistry みたいな専門分野のスキルもあるのは、コミュニティの多様性を感じる

#### Databases カテゴリ（1,543 スキル）

データベース関連のスキルが 1,543 個。SQL の書き方とか、各データベースの特性を活かしたクエリの最適化とか、そういう知識がスキルとして使える

### Skills の仕組みを理解しよう

Skills Marketplace を使いこなすには、Skills 自体の仕組みを理解しておくと良い。ドキュメントページにわかりやすくまとまってるから、そこから抜粋して解説する

#### Token Efficiency（トークン効率）

Skills は lazy loading を採用してて、トークンとコストを節約できる仕組みになってる

- Claude は最初、スキルの名前と説明だけを見る
- 現在のタスクに関連する場合のみ、全コンテンツをロードする
- 使われないスキルは会話でトークンを消費しない

これがめっちゃ重要で、大量のスキルをインストールしても、必要なときだけロードされるから効率的なんよね

#### Domain Expertise（専門知識の注入）

Skills は専門知識を注入する複数のアプローチを提供してる

- Professional knowledge injection: ベストプラクティスを SKILL.md に直接記述
- Operational consistency: 毎回安定した品質を確保
- Reference docs: 必要なときにロードされる詳細ドキュメント（references/ に配置）
- Resource files: テンプレートやアセット（assets/ ディレクトリに配置）

これによって、単なる「指示」じゃなくて「専門家の知識」を Claude に取り込めるわけ

### Skills と MCP の違いを整理しよう

ここが一番大事な部分かもしれない。Skills と MCP（Model Context Protocol）は根本的に違う問題を解決してる

#### Skills の特徴

- **知識共有**: 経験、ベストプラクティス、ワークフローを共有
- **シンプルな Markdown ファイル**: 誰でも作成できる
- **Progressive loading**: トークン効率が良い
- **サーバー不要**: セットアップの手間がない
- **クロスプラットフォーム**: Web + Desktop + CLI で動作（ほとんどのスキル）

#### MCP の特徴

- **機能拡張**: API、データベース、ツールへの接続
- **コーディングとサーバー設定が必要**: 技術的なハードルが高い
- **起動時に全ツール定義をロード**: トークン消費が高い
- **外部統合に強力**: 実際のシステムと連携できる
- **複雑なセットアップ**: 導入コストが高い

Skills と MCP は補完関係にある。知識共有には Skills を使い、機能拡張には MCP を使う。両方を組み合わせると本当の力を発揮する

### 実際の使い方

Skills Marketplace の使い方は簡単

#### AI セマンティクス検索で探す

トップページの検索バーに「やりたいこと」を入力するだけ。自然言語で検索できるから、キーワードを考える必要がない

例えば「React コンポーネントのテストを書きたい」と入力すると、関連するスキルがヒットする

#### カテゴリから探す

カテゴリページ（/categories）から探すのも効率的。自分の目的に合ったカテゴリを選んで、サブカテゴリを絞り込んでいく

全部で 65,000 以上あるから、最初はカテゴリから探す方が見つけやすいかもしれない

#### 人気順でソート

stats ページで人気のスキルを確認できる。多くの人に使われてるスキルは品質が高い傾向にあるから、まずは人気のものから試してみるのもあり

### スキルのインストール方法

見つけたスキルをインストールするには、GitHub リポジトリをクローンして、.claude/skills/ ディレクトリに配置するだけ

.claude/skills/example-skill/SKILL.md

```bash
---

name: xxxx

description: スキルの説明

---

# Example Skill

ここにスキルの内容が書かれている
```

SKILL.md 標準に準拠してるから、どのスキルも同じ構造になってて扱いやすい

### なぜ今 Skills Marketplace が重要なのか

ここまで読んで「へー、便利そうだな」と思ってくれたならうれしい。でも、なぜ「今」これが重要なのかをもう少し掘り下げたい

#### 車輪の再発明を避ける

自分でスキルを一から作る前に、まずは既存のスキルを探すべき。65,000 以上あるから、大抵のユースケースはカバーされてる

見つかったスキルをベースに、自分用にカスタマイズする方が効率的やし、品質も高くなりやすい

#### 公式からの推奨

Anthropic 公式がこのサービスを紹介してるという事実は大きい。信頼性の裏付けになるし、今後もサポートされていく可能性が高い

LangChain の deepagent-CLI にも統合されてるから、エコシステムとしての広がりも期待できる

### 自分でスキルを作る場合

もちろん、既存のスキルでカバーできないユースケースもある。そういうときは自分でスキルを作ることになる

### サイトの UI/UX がめっちゃ開発者向け

Skills Marketplace のサイトデザインも特徴的なんよね。ターミナル風の UI になってて、開発者にとっては親しみやすい

トップページには `const skills = 65,635;` みたいなコード風の表示があったり、カテゴリページでは `export Tools` みたいな見出しになってたりする。遊び心があっていい

検索バーは `$ ai --search` というコマンド風のプロンプトになってて、ここにやりたいことを自然言語で入力すると AI が関連スキルを探してくれる

### 実際に触ってみて感じたこと

ワイが実際にサイトを触ってみて感じたことを正直に書いておく

#### 良かった点

まず検索性が高い。65,000 以上あると探すの大変そうに思えるけど、カテゴリ分けがしっかりしてるから目的のスキルにたどり着きやすい。AI セマンティクス検索も精度が高くて、「こういうことしたい」という曖昧な入力でもちゃんと関連スキルが出てくる

全部 GitHub のオープンソースっていうのも安心感がある。スキルの中身を事前に確認できるし、何か問題があればイシューを立てたりプルリク送ったりできる。クローズドなマーケットプレイスだとこうはいかない

カテゴリの粒度もちょうどいい。大カテゴリが 10 個、サブカテゴリがそれぞれ 4〜11 個くらいで、多すぎず少なすぎず探しやすい

#### 改善してほしい点

スキルの品質にばらつきがあるのは仕方ないけど、もう少しキュレーションがあると助かる。人気順でソートできるとはいえ、品質保証されたスキルをまとめたセクションとかあると初心者には優しいかも

あと日本語対応がまだないから、英語が苦手な人にはちょっとハードルが高い。スキル自体は英語で書かれてることが多いけど、サイトの UI は多言語対応してほしい気持ちはある

### おすすめの使い方

ここからは実践的な使い方を紹介する

#### 新しいフレームワークを学ぶとき

例えば React から Vue に乗り換えようとしてるとき、Vue のベストプラクティスを教えてくれるスキルをインストールしておくと、Claude がそのスキルを参照しながらコードを書いてくれる

フレームワークの公式ドキュメントを読むのも大事やけど、実践的なノウハウはスキルから得る方が早いことも多い

#### チーム開発の標準化

チームで Claude Code を使う場合、同じスキルセットを共有することで出力の一貫性が保たれる。コーディング規約とかコミットメッセージのルールとか、チームのスタンダードをスキルとして定義しておくと便利

Skills Marketplace から見つけたスキルをベースに、チーム独自のカスタマイズを加えるのもあり

#### 専門外の領域に挑戦するとき

普段バックエンドをやってる人がフロントエンドに挑戦するとき、Frontend カテゴリのスキルを入れておくと心強い。逆もまた然り

専門家が作ったスキルを使うことで、その領域のベストプラクティスを自然と取り入れられる

### 今後の展望

Skills Marketplace は今後さらに成長していくと予想してる

まずスキル数が増え続けるのは確実。12 月末から急増してるトレンドを見ると、コミュニティの勢いは止まらない

Anthropic 公式が紹介してることからも、Skills エコシステム自体がサポートされ続ける可能性は高い。Claude Code のアップデートに合わせて、スキルの仕様も進化していくやろう

MCP との連携も興味深い。現時点では Skills と MCP は別々のものやけど、将来的には両者を組み合わせたワークフローがもっと洗練されていくかもしれない

### まとめ

Agent Skills Marketplace は Claude Code ユーザーにとって必須のリソースになりつつある

- 65,635 個以上のオープンソーススキル
- SKILL.md 標準に準拠
- AI セマンティクス検索で探しやすい
- Anthropic 公式も紹介
- Skills と MCP は補完関係

スキルを自作する前に、まずはここをチェックしてみてほしい。先人たちの知恵を借りることで、Claude Code の活用が一気に加速するはず

リンク: [Agent Skills Marketplace](https://skillsmp.com/)

### 参考リンク

- [Agent Skills Marketplace](https://skillsmp.com/)
- [Agent Skills Open Specification](https://skillsmp.com/docs)
- [Claude Platform Documentation - Agent Skills Overview](https://docs.anthropic.com/en/docs/claude-code/skills)
- [Equipping agents for the real world - Anthropic Blog](https://www.anthropic.com/news/equipping-agents-for-the-real-world)

Insights （意見・フィードバック）