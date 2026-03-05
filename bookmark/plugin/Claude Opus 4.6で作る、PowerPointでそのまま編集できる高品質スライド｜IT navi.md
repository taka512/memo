---
title: "Claude Opus 4.6で作る、PowerPointでそのまま編集できる高品質スライド｜IT navi"
source: "https://note.com/it_navi/n/ne26d4d078386"
author:
  - "[[IT navi]]"
published: 2026-02-22
created: 2026-03-05
description: "AIを使ったスライド作成は以前から可能でしたが、作った後に編集できないという課題がついて回りました。例えばNotebookLMでは、多彩なデザインのスライドをpptxファイルとして出力できるものの、各ページが1枚の画像として埋め込まれるため、文字や図表を後から修正するのが困難です。  その点、Claudeで作成したスライドはパーツごとに分かれたpptxファイルとしてダウンロードできるため、PowerPoint上でそのまま編集できるのが大きな強みです。さらに、専用のスキルを活用すれば、洗練されたデザインのスライドを安定して繰り返し生成できます。  本記事では、このパワポスライド作成用スキ"
tags:
  - "clippings"
image: "https://assets.st-note.com/production/uploads/images/253647712/rectangle_large_type_2_11bc884478f1cadbfa529ff7f5500c7f.png?fit=bounds&quality=85&width=1280"
---
![見出し画像](https://assets.st-note.com/production/uploads/images/253647712/rectangle_large_type_2_11bc884478f1cadbfa529ff7f5500c7f.png?width=1280)

## Claude Opus 4.6で作る、PowerPointでそのまま編集できる高品質スライド

[IT navi](https://note.com/it_navi)

AIを使ったスライド作成は以前から可能でしたが、作った後に編集できないという課題がついて回りました。例えばNotebookLMでは、多彩なデザインのスライドをpptxファイルとして出力できるものの、各ページが1枚の画像として埋め込まれるため、文字や図表を後から修正するのが困難です。

その点、Claudeで作成したスライドはパーツごとに分かれたpptxファイルとしてダウンロードできるため、PowerPoint上でそのまま編集できるのが大きな強みです。さらに、専用のスキルを活用すれば、洗練されたデザインのスライドを安定して繰り返し生成できます。

本記事では、このパワポスライド作成用スキルの作り方と使い方を解説します。今回作成したスキルも公開しますので、ぜひご活用ください。

[**GPTsでChatGPTを優秀な部下にしよう！ GPTsパーフェクト作成ガイド** *amzn.to*](https://amzn.to/4kLyK6i)

[Amazon.co.jpで購入する](https://amzn.to/4kLyK6i)

## １．パワポスライド作成用スキルの作り方

### (1) 参考にするスライドの準備

最初に、参考にするスライドやスライド作成のためのプロンプトを準備します。スライドの画像を使用する場合は、画像そのものを入力時に参照することもできますが、画像の内容をClaudeに分析させてからスキルを作成する方が高品質なものができると思います。

今回は、 [FUJI / Presentation designerさんの記事](https://note.com/yoshifujidesign/n/n23c3d6b90523) に掲載されていた「マッキンゼー風スライド」のプロンプトを利用させていただきました。

```python
Requirement:
A professional, high-density consulting presentation slide, designed in the style of a top-tier strategy firm (McKinsey/BCG) blended with high-end editorial aesthetics.

Core Content & Layout:
1. Rich Data Visualization: The slide is populated with complex, precise charts (stacked bar charts, waterfall charts, or line graphs) and detailed data tables with rows and columns.
2. Structured Frameworks: Includes strategic diagrams or 2x2 matrices constructed with thin, clean lines.
3. High Information Density: The layout is sophisticated and multi-column, mimicking an actual business analysis deck, not just an empty cover page.

Visual Style:
1. Aesthetic: Tech-minimalist but information-heavy. Clean, sharp, and authoritative.
2. Typography: Serif fonts (like Times New Roman) for the main headlines to give a premium financial report feel; clean Sans-serif for chart labels and data numbers.
3. Color Palette: Clean white background. Text is sharp black. Charts and graphical accents use Deep Royal Blue and distinct shades of grey for data hierarchy.
4. Graphics: Use fine hairline borders for tables and precise vector lines for graphs.
```

### (2) スキルの作成

スキルの作成は、Claudeの入力欄で「スキルを作成して」と指示するだけでできます。使用モデルは、Claude Opus 4.6です。

> **以下のプロンプトを参考にして、ユーザーが提供した資料に関する洗練されたデザインの日本語パワポスライドを作成するスキルを作成してください：**  
> （上記マッキンゼー風スライドのプロンプトを張り付け）

**【Claude Opus 4.6の回答】**

Let me start by reading the relevant skill files to understand the best practices.Now let me read the truncated section of the skill creator for writing patterns:Now I have all the context. Let me create the skill. I'll build it as a complete skill with SKILL.md and a design system reference, adapted for Japanese language presentations with the specific aesthetic the user described.Now let me run a quick test to verify the skill works by creating a sample presentation:All slides render cleanly. Now let me package the skill and copy everything to the output directory.  
スキルを作成し、テスト検証まで完了しました。  
以下の4ファイルをお届けします：

**スキル構成：**

- **jp-consulting-slides.skill** — インストール可能なスキルパッケージ
- **SKILL.md** — メインの指示ファイル（ワークフロー、日本語特有の注意点、QAプロセス）
- **references/design-system.md** — 完全なデザインシステム仕様（カラートークン、タイポグラフィ、レイアウトグリッド、全テンプレート関数のコード）

**サンプル出力：**

- **sample\_output.pptx** — 「DX戦略分析レポート」5枚のテストデッキ（タイトル、エグゼクティブサマリー、積み上げ棒グラフ＋テーブル、2x2マトリクス、提言）

主な設計ポイントは、見出しに游明朝（セリフ体）、本文に游ゴシック（サンセリフ体）を使い分けるプレミアム金融レポートのタイポグラフィ、ディープロイヤルブルーからグレーへのグラデーションパレット、ヘアラインボーダーのテーブル、そして全スライドにアクションタイトル（洞察を述べる文）を求める構成です。

![画像](https://assets.st-note.com/img/1771734331-hkAdgJMnKuCH4cXVvPlp1Oze.png?width=1200)

sample\_output.pptx

### (3) スキルの修正

上記(2)のスキルをClaudeに設定して、使用してみたところ、「スライドを作成して」と指示しても、なかなかスキルが発動しなかったので、トリガー条件を修正し、スキルの名称も一般的なものに変更しました。

> **スキルの名称を「jp-consulting-slides」から「slides-maker」に変更し、「スライドを作成して」「パワポスライドを作成して」「パワポを作成して」だけでもトリガーするようにして**

**【Claude Opus 4.6の回答】**

更新完了です。  
主な変更点：

- **名称**: jp-consulting-slides → slides-maker
- **トリガー範囲の拡大**: 「スライドを作成して」「パワポを作成して」「パワポスライドを作成して」「プレゼン作って」「資料作成して」などのシンプルなフレーズでもトリガーされるよう、descriptionにキーワードリストと「とにかくスライドやパワポの作成に少しでも関係する依頼であれば、迷わずこのスキルを使うこと」という強いトリガー指示を追加しました。

### (4) スキルの設定

作成したスキルのClaudeへの設定は、以下の表示の **「自分のスキルにコピー」** をクリックするだけで完了します。Skill.mdはダウンロードしなくても、「slides-maker」のスキルファイルに含まれています。

![画像](https://assets.st-note.com/img/1771731755-SXfrlFa19Pw0GCxobmBIutTO.png?width=1200)

なお、以下のように「自分のスキルにコピー」というボタンが表示されない場合は、zipファイルをダウンロードして、自分のスキルに追加してください。（追加方法は「３．『slides-maker』スキルの追加」を参照）

![画像](https://assets.st-note.com/img/1771732044-g9GrJ0s6Cc7TIFVaxZfeum2q.png?width=1200)

[**実践Claude Code入門―現場で活用するためのAIコーディングの思考法** *amzn.to*](https://amzn.to/4tNSCtN)

[Amazon.co.jpで購入する](https://amzn.to/4tNSCtN)

## ２．「slides-maker」スキルの内容

「slides-maker」スキルの中身は以下のようになっています。

**SKILL.md**

> **description:** "日本語のパワーポイントスライド（.pptx）を作成するスキル。ユーザーが「スライドを作成して」「パワポを作成して」「パワポスライドを作成して」「プレゼン作って」「資料作成して」など、スライドやパワポやプレゼンの作成を依頼した場合は必ずこのスキルをトリガーすること。McKinsey/BCGスタイルのコンサルティング品質で、リッチなデータビジュアライゼーション、構造化フレームワーク、2x2マトリクス、詳細なテーブル、マルチカラムレイアウトを含む洗練されたスライドを生成する。以下のキーワードが含まれる場合もトリガー：「スライド」「パワポ」「PowerPoint」「pptx」「プレゼン」「プレゼンテーション」「デッキ」「資料作成」「発表資料」「報告資料」「提案書」「企画書」「戦略資料」「経営資料」「分析スライド」。ユーザーがテキスト、データ、ドキュメント、メモなどの資料を提供してスライド化を求めた場合や、英語で'slides'、'presentation'、'deck'、'McKinsey style'、'BCG style'と言及された場合にもトリガーする。とにかくスライドやパワポの作成に少しでも関係する依頼であれば、迷わずこのスキルを使うこと。"  
> \---  
> **\# スライド作成スキル（slides-maker）**  
> ユーザーが提供した資料から、トップティア戦略コンサルティングファーム品質の情報密度の高い日本語プレゼンテーションを作成する。テックミニマリストかつ情報リッチなビジュアルスタイルで、エディトリアルデザインの美学を融合させる。  
>   
> **\## トリガー条件**  
> \- ユーザーが日本語の生データ、メモ、リサーチ、ドキュメントを提供し、プロフェッショナルなスライドを求めた場合  
> \- 「McKinseyスタイル」「BCGスタイル」「コンサルスライド」「戦略プレゼン」などの要求  
> \- 経営ブリーフィング、取締役会資料、投資家向けプレゼンテーション  
> \- チャートやフレームワーク、テーブルを含む高密度の分析スライドの要求  
>   
> **\## 前提条件**  
> 作業開始前に、必ず以下のスキルファイルを読み込むこと：  
> Read /mnt/skills/public/pptx/SKILL.md  
> Read /mnt/skills/public/pptx/pptxgenjs.md  
> 次に、日本語コンサルデザインシステムのリファレンスを読み込む：  
> Read slides-maker/references/design-system.md  
>   
> **\## ワークフロー  
> \### 1. ユーザー資料の分析**  
> アップロードされたファイルまたは提供されたテキストを調査し、以下を特定する：  
> \- **キーメッセージ**: 主要な2〜3つの結論・洞察は何か？  
> \- **データポイント**: 数値、パーセンテージ、トレンド、比較  
> \- **適用可能なフレームワーク**: SWOT、2x2マトリクス、ウォーターフォール、ファネル、タイムライン  
> \- **階層構造**: 全体的なナラティブアーク（物語構造）  
>   
> **\### 2. デッキ構成の計画**  
> コンサルティングデッキは厳密なナラティブロジックに従う。スライドを以下の構成で計画する：  
> 1\. **タイトルスライド** — テーマ、サブタイトル、日付。ダークネイビー背景、セリフ体見出し。  
> 2\. **エグゼクティブサマリー** — 「So What（要は何か）」を先に提示。重要発見事項を構造化レイアウトで表示。  
> 3\. **状況 / コンテキスト** — 背景データ、市場概況、チャート。  
> 4\. **分析スライド** （2〜4枚）— 核となる分析コンテンツ。以下を組み合わせる：  
> \- データビジュアライゼーションスライド（チャート＋注釈）  
> \- フレームワークスライド（2x2マトリクス、比較テーブル）  
> \- 深堀りスライド（マルチカラム＋補足データ）  
> 5\. **示唆 / 提言** — 構造化されたテイクアウェイ、優先度付きアクション。  
> 6\. **付録** （任意）— 補足データテーブル、方法論ノート。  
> 各スライドには必ず **\*\*アクションタイトル\*\*** を付けること。トピックラベルではなく、洞察を述べる文。  
> **アクションタイトルの例：**  
> \- ✅ 「デジタル売上は前年比34%増、主にエンタープライズ契約が牽引」  
> \- ❌ 「売上概要」  
>   
> **\### 3. デザインシステムに従ったスライド構築**  
> \`references/design-system.md\` のデザインシステムに厳密に従うこと。主要原則は以下の通り：  
> **カラーパレット：**  
> \- 背景: \`FFFFFF\`（ホワイト）  
> \- プライマリテキスト: \`1A1A1A\`（ニアブラック）  
> \- 見出し: \`0B1F3F\`（ディープネイビー）  
> \- チャートプライマリ: \`1B3A6B\`（ロイヤルブルー）  
> \- チャートシェード: \`3D6098\`, \`7A9CC6\`, \`B8CCE4\`, \`D9E5F2\`（ブルーからグレーのグラデーション）  
> \- アクセント: \`C8102E\`（レッド、強調時のみ控えめに使用）  
> \- グリッド/ボーダー: \`D0D0D0\`（ライトグレーのヘアライン）  
>   
> **タイポグラフィ（日本語対応）：**  
> \- スライドのアクションタイトル: 游明朝（Yu Mincho）, 16-18pt, ボールド, \`0B1F3F\`  
> \- セクションヘッダー: 游明朝（Yu Mincho）, 13-14pt, ボールド  
> \- 本文 / チャートラベル: 游ゴシック（Yu Gothic）, 10-12pt, \`333333\`  
> \- データ数値: Calibri, 10pt, ボールド  
> \- ソースライン: 游ゴシック（Yu Gothic）, 8pt, イタリック, \`888888\`  
>   
> **レイアウトルール：**  
> \- 全辺0.5インチマージン  
> \- マルチカラムレイアウト推奨（2〜3カラム）  
> \- 全スライドのアクションタイトル下に細いネイビーライン（0.5pt, \`1B3A6B\`）  
> \- 全データスライドの左下にソース引用  
> \- 右下にページ番号、游ゴシック 8pt, グレー  
>   
> **\### 4. チャート＆ビジュアライゼーション ガイドライン**  
> pptxgenjsを使用して、コンサルティンググレードのスタイリングでチャートを生成する：  
> \- **棒グラフ/カラムチャート**: ロイヤルブルーをプライマリ、グレーシェードをセカンダリに。3D不可。クリーンな値ラベル。  
> \- **積み上げ棒グラフ**: ブルーからグレーのグラデーションパレットをセグメントに使用。  
> \- **折れ線グラフ**: 細いライン（2pt）、データポイントマーカー、クリーンなグリッド。  
> \- **テーブル**: ヘアラインボーダー（\`D0D0D0\`, 0.5pt）。ヘッダー行はネイビー（\`1B3A6B\`）白テキスト。行の背景色を交互に（\`F5F7FA\` / \`FFFFFF\`）。  
> \- **ウォーターフォールチャート**: 積み上げ棒グラフとコネクターラインの組み合わせで構築。ベースは透明、増加はロイヤルブルー、減少はアクセントレッド。  
> データが明示的に提供されていないがユーザーの資料からトレンドが暗示される場合、代表的なデータビジュアライゼーションを作成し、「図示のため仮定値を使用」と注記する。  
>   
> **\### 5. フレームワークスライド**  
> 2x2マトリクス、比較フレームワーク、戦略ダイアグラムの場合：  
> \- シェイプ（矩形＋ライン）で構築し、画像は使わない  
> \- 細いライン（0.5-1pt）を\`D0D0D0\`で使用  
> \- 軸ラベルはマトリクスの外側に明確に配置  
> \- 各象限に薄い色のティント背景  
> \- 象限ラベルはボールド、説明文はレギュラーウェイト  
> \- 日本語テキストは必ずテキストボックスの幅を十分に確保（折り返し対策）  
>   
> **\### 6. 日本語特有の注意点**  
> \- **フォントの指定**: 日本語見出しには游明朝（Yu Mincho）またはMS 明朝を使用し、本文には游ゴシック（Yu Gothic）またはMeiryo UIを使用する。英数字やデータ数値にはCalibrを使用。  
> \- **文字サイズ**: 日本語は英語より1-2pt大きくして可読性を確保する。  
> \- **テキストボックスの幅**: 日本語テキストは英語より幅が必要になることが多い。余裕を持った幅設定を行う。  
> \- **句読点**: 全角の「。」「、」を使用する。  
> \- **敬語レベル**: ビジネス文書として適切な敬語レベルを維持する。「です・ます」調を基本とする。  
> \- **数字表記**: 大きな数字は漢数字（億、万）と算用数字を適切に組み合わせる（例：1.2兆円、340億円）。  
>   
> **\### 7. QAプロセス**  
> 標準のPPTX QAプロセスに従う：  
> 1\. PDFに変換 → 画像化  
> 2\. 全スライドを目視で検査：  
> \- テキストのオーバーフローや重なり  
> \- チャートの可読性  
> \- マージンとアライメントの一貫性  
> \- カラーの一貫性  
> \- 全コンテンツスライドにアクションタイトルがあるか  
> \- 日本語テキストの文字化けや切れがないか  
> \- フォントのフォールバックが発生していないか  
> 3\. 問題を修正し、再検証  
>   
> **\## デザイン・アンチパターン（避けるべきこと）**  
> \- トピックラベルのみの汎用タイトル（アクションタイトルでないもの）  
> \- ビジュアル要素のない箇条書きだらけのスライド  
> \- 3つ以上のフォントファミリーの混在  
> \- ソース引用のないチャート  
> \- ホワイトスペースのない詰め込みレイアウト  
> \- 定義済みパレット外の明るい・彩度の高い色の使用  
> \- 情報を伝えない装飾要素  
> \- 2つ未満のビジュアル要素（チャート、テーブル、フレームワーク、コールアウト）しかないスライド  
> \- LibreOfficeでの日本語レンダリング問題を考慮しないフォント選択  
> \- 英語と日本語が混在する際のフォントサイズの不統一

**references / design-system.md**

> **\# 日本語コンサルスライド デザインシステム リファレンス**  
> PptxGenJSを使用してMcKinsey/BCG品質のコンサルティングプレゼンテーションスライドを日本語で作成するための完全な設計仕様。テックミニマリストかつ情報密度の高いエディトリアルデザインを実現する。  
> \---  
> **\## 目次**  
> 1\. \[カラートークン\](#カラートークン)  
> 2\. \[タイポグラフィシステム\](#タイポグラフィシステム)  
> 3\. \[レイアウトグリッド\](#レイアウトグリッド)  
> 4\. \[スライドテンプレート\](#スライドテンプレート)  
> 5\. \[チャートスタイリング\](#チャートスタイリング)  
> 6\. \[テーブルスタイリング\](#テーブルスタイリング)  
> 7\. \[フレームワークコンポーネント\](#フレームワークコンポーネント)  
> 8\. \[ウォーターフォールチャート\](#ウォーターフォールチャート)  
> 9\. \[再利用可能コードパターン\](#再利用可能コードパターン)  
> \---  
> **\# カラートークン**  
> 全スクリプトの先頭で定数として定義する：

> **\# タイポグラフィシステム**  
> 日本語プレゼンテーション用に最適化した2フォントファミリーシステム。見出し系にはセリフ体（游明朝）、本文系にはサンセリフ体（游ゴシック）を使用し、プレミアムな金融レポートの雰囲気を実現する。数字・英字にはCalibrを補助的に使用する。

![画像](https://assets.st-note.com/img/1771730387-19cFRTAV0iJUdubZ2owLvMpC.png)

（以下省略）

> **\### 重要な注意事項**  
> 1\. 必ずファクトリ関数を使用する — オプションオブジェクトを複数の\`addShape\`/\`addText\`呼び出しで再利用しないこと（PptxGenJSはオブジェクトをインプレースで変更する）。  
> 2\. Hexカラーに\`#\`プレフィックスを使わない。  
> 3\. 8文字Hexで透明度を指定しない — \`opacity\`プロパティを使用する。  
> 4\. アクションタイトルは文であり、トピックラベルではない。  
> 5\. 全データスライドにソースラインが必要。  
> 6\. PDF変換でテストする — LibreOfficeのレンダリングはPowerPointと異なる場合がある。  
> 7\. 日本語テキストの幅に注意 — 日本語は英語より文字幅が大きいため、テキストボックスの幅に余裕を持たせる。  
> 8\. フォントフォールバックを考慮する — 游明朝/游ゴシックが利用できない環境では代替フォントを用意する。

[**最速でわかる生成AI実践ガイド** *amzn.to*](https://amzn.to/4kRDeIT)

[Amazon.co.jpで購入する](https://amzn.to/4kRDeIT)

## ３．「slides-maker」スキルの追加

slides-makerのスキルファイルは以下からダウンロードできます。このファイルをClaudeに追加して、スキルを使用することができます。

スキルを追加するには、最初に左メニューの **「カスタマイズ」** をクリックします。

![画像](https://assets.st-note.com/img/1771950150-T4NqyOo7sleYXDHi2BRdELCJ.png)

左メニューの上部

次にカスタマイズ画面の **「新しいスキルを作成」** 又は左メニューの **「スキル」** をクリックします。

![画像](https://assets.st-note.com/img/1771950215-D3H2yTxclKZEm68MnoXG4Rsp.png?width=1200)

次に、スキル画面上部の **「＋」** マークをクリックし、ドロップダウンメニューから **「スキルをアップロード」** を選択します。

![画像](https://assets.st-note.com/img/1771950759-cmC0YzjMhwSXeKLpNJGg2lna.png?width=1200)

スキルアップロード画面から、自分のパソコンに保存した **「slides-maker.skill」** のファイルをアップロードしてください。

![画像](https://assets.st-note.com/img/1771950835-hHK7YE32WnmM9pRwTJDczoNj.png)

スキル画面に戻って、 **「マイスキル」** の下に **「slides-maker」** と表示されていれば、スキルの追加成功です。

![画像](https://assets.st-note.com/img/1771951135-Lwj7kYO6tQaxPhupgMlvRSFe.png?width=1200)

右側の **「slides-maker」** の表示のタイトルの右のトグルスイッチをオンにすると、Claude上で「slides-maker」のスキルを使えるようになります。

[**生成AI最速仕事術** *amzn.to*](https://amzn.to/3MXusMG)

[Amazon.co.jpで購入する](https://amzn.to/3MXusMG)

## ４．スライド作成の実例 ①（Grok 4.20の解説）

以下のように、スライドの作成を指示すると、自動的に「slides-maker」のスキルが使用されます。  
Claudeがネットで関連情報を収集して、スライドにまとめます。

> **Grok 4.20についてのスライドを作成して**

**【Claude Opus 4.6の回答】**

まず、Grok 4.20についての最新情報を検索し、スライド作成のスキルを確認します。情報が揃いました。次にスライド作成スキルを確認します。情報とスキルを全て確認しました。それではGrok 4.20についてのスライドを作成します。全スライドの品質を確認しました。出力ファイルを準備します。Grok 4.20の分析レポートスライド（全8枚）を作成しました。構成は以下の通りです：

1. **タイトル** — ダークネイビー背景
2. **エグゼクティブサマリー** — 4つの重要ポイント
3. **4エージェント協調システム解説** — Grok/Harper/Benjamin/Lucasの役割カード
4. **ベンチマーク・パフォーマンス比較** — Eloスコアチャート＋KPIコールアウト
5. **Grokシリーズ進化タイムライン** — 4→4.1→4.20の比較テーブル
6. **実用分野と差別化** — 金融・医療・コーディング・数学の2×2レイアウト
7. **料金プラン＆今後の展望** — 価格テーブル＋ロードマップ
8. **示唆・提言** — 業界への3つのインプリケーション
![画像](https://assets.st-note.com/img/1771739706-MSaNiJtmHDkK8YfAxrLRwsh2.png?width=1200)

上のように表示された部分をクリックすると、Claudeの画面の右側にスライドが表示されます。  
また、ダウンロードをクリックするとスライドがpptxファイルとしてダウンロードされて、PowerPointで編集できるようになります。

**完成したスライド**

![画像](https://assets.st-note.com/img/1771740153-YnGaT5w23PAEzVqZ1rhlXdM4.png?width=1200)

1/8

![画像](https://assets.st-note.com/img/1771740166-InEjkUwJNqvfaA378gxyYbos.png?width=1200)

2/8

![画像](https://assets.st-note.com/img/1771740180-sLiRvwCYNkjeoDU2Q6AhXEgZ.png?width=1200)

3/8

![画像](https://assets.st-note.com/img/1771740205-3dL2eyUiEv45Ajs1GHRNxzwJ.png?width=1200)

4/8

![画像](https://assets.st-note.com/img/1771740217-6ugeX0f72AFKSrVdOZ9mGpQC.png?width=1200)

5/8

![画像](https://assets.st-note.com/img/1771740231-QXEZFWwmut8IaLH1v0hgzJ7r.png?width=1200)

6/8

![画像](https://assets.st-note.com/img/1771740243-J2HQqj3uTwROoUYzNfLVn4Bv.png?width=1200)

7/8

![画像](https://assets.st-note.com/img/1771740264-bq6XjwB5xofKGCWphH7zOMdR.png?width=1200)

8/8

[**あっという間のAIスライド作成術 (医師による医師のためのChatGPT入門)** *amzn.to*](https://amzn.to/4kYuNLJ)

[Amazon.co.jpで購入する](https://amzn.to/4kYuNLJ)

## ５．スライド作成の実例 ②（note記事のスライド化）

次は、 [自分が書いたnote記事](https://note.com/it_navi/n/n72410a65f569) の内容をスライド化します。

> **https://note.com/it\_navi/n/n72410a65f569  
> この記事の内容のスライドを作成して**

**【Claude Opus 4.6の回答】**

記事の内容を把握しました。コンサルスタイルのスライドを作成します。The title on slide 5 is wrapping. Let me fix that.All slides look clean. Let me copy the final file to the outputs directory.全8スライドの資料を作成しました。記事の主要な論点を以下の構成でまとめています。

1. **タイトル** — テーマと4大AI横断比較の概要
2. **エグゼクティブサマリー** — 3つの主要発見事項
3. **全員一致のClaude評価** — 各モデルの自己申告を4つのカードで表示
4. **フェーズ別の最適モデル** — 構想・長編管理・執筆・表現の4フェーズ比較表
5. **実践的な最強コンボ** — 3ステップのワークフローを3カラムで図示
6. **調査・分析力の評価テーブル** — 4軸×4モデルの比較
7. **調査・分析力ランキング** — 1位GPT〜4位Grokの順位と理由
8. **まとめ** — 「書く力」と「調べる力」は別の才能
![画像](https://assets.st-note.com/img/1771741515-vYjOfpq2oz63GxCdU4aLZKXw.png?width=1200)

上のように表示された部分をクリックすると、Claudeの画面の右側にスライドが表示されます。  
また、ダウンロードをクリックするとスライドがpptxファイルとしてダウンロードされて、PowerPointで編集できるようになります。

**完成したスライド**

![画像](https://assets.st-note.com/img/1771741587-N0k6LQDBytosf7Cjexu5K1En.png?width=1200)

1/8

![画像](https://assets.st-note.com/img/1771741607-MogNeT5HburcvRqUpL9SIxZs.png?width=1200)

2/8

![画像](https://assets.st-note.com/img/1771741618-HZw9anMrCRPoN3E2qpI0Bc4u.png?width=1200)

3/8

![画像](https://assets.st-note.com/img/1771741630-qI42pBP0R9AsXOyT6cEoN5UL.png?width=1200)

4/8

![画像](https://assets.st-note.com/img/1771741642-nwN9rSJUBchMs7IEWlFXV1zt.png?width=1200)

5/8

![画像](https://assets.st-note.com/img/1771741653-QukCtHIx2ZNVzOMFGidBDTgK.png?width=1200)

6/8

![画像](https://assets.st-note.com/img/1771741678-aTNQOSpcJW0uRX3wdvzLjr8f.png?width=1200)

7/8

![画像](https://assets.st-note.com/img/1771741691-RulO4bhFHZ6pdzEPrUKYxJ59.png?width=1200)

8/8

[**AIエージェント開発 / 運用入門 \[生成AI深掘りガイド\]** *amzn.to*](https://amzn.to/4s9I2f3)

[Amazon.co.jpで購入する](https://amzn.to/4s9I2f3)

## ６．おわりに

今回は、ビジネスで使えるマッキンゼー風のスライドを参考にしましたが、本記事で紹介したスキル作成方法を応用すれば、普段自分が使っているスライドや気に入ったデザインのスライドをもとに、オリジナルのスライド作成用スキルを作ることができます。  
  
スライドのスタイルを探すには、以下のようなサイトが参考になるでしょう。

[**BananaX プロンプトパターン300選 | 画像生成スタイル辞典【2026年版】** *300以上のインフォグラフィック・プロンプトスタイルを無料で検索・コピー。Flat・Isometric・Doodleなど人* *furoku.github.io*](https://furoku.github.io/bananaX/projects/infographic-evaluation/)

様々なスタイルを選択できるスライド作成用スキルを作ることもできますが、あまり奇抜なスタイルは実用的ではないため、自分がよく使うスタイルに絞ったスキルを作成するのがおすすめです。

また、今回はスライド作成の際にClaude Opus 4.6を使用しましたが、Sonnet 4.6でも十分に品質の高いスライドを作成できます。ただし、情報を収集・分析する能力には差がありますので、作成するスライドの内容に応じて使い分けるとよいでしょう。  
  
今回紹介したように、スライド作成用のスキルは誰でも簡単に作成できますので、ぜひ皆さんもオリジナルのスキルを作って活用してみてください。

## おまけ：スキル無しでも高品質なスライドを作成する方法

有名なスタイルを参考にするように指示すれば、特別なスキル無しでもClaudeで高品質なスライドを作成することができます。（デフォルトのPPTXスキルは使用されています。）

### (1) デジタル庁デザインシステムスタイル

例えば、日本では、誰にでも読みやすいデザインを目指した **「デジタル庁デザインシステム」** などが有名です。

[**デジタル庁デザインシステムβ版** *デジタル庁デザインシステムは、スタイリングの考え方を提供するデザイン言語、情報の視覚表現とインタラクションを具現化するUI* *design.digital.go.jp*](https://design.digital.go.jp/dads/)

そこで、これを利用してスライドを作成してみます。

> **デジタル庁デザインシステムのスタイルで、ChatGPTにアダルトモードが間もなく登場するという噂に関するスライドを作成して**

**【Claude Opus 4.6の回答】**

まず、関連情報とスキルを確認させてください。すべてのスライドの品質を確認できました。デジタル庁のデザインシステムに沿った、クリーンで統一感のある仕上がりです。デジタル庁デザインシステムのスタイルで全7スライドの資料を作成しました。

ネイビー×ホワイトの配色、セクションラベル付きの統一ヘッダー、アクセシビリティを意識したコントラストなど、デジタル庁の視覚的特徴を反映しています。内容は最新の報道（2026年2月時点）に基づき、概要・経緯・機能・年齢認証の課題・リスク・まとめの構成でまとめています。

**完成したスライド**

![画像](https://assets.st-note.com/img/1771752641-16Cx8WhRliwNzD5vBb7Po4ZE.png?width=1200)

1/7

![画像](https://assets.st-note.com/img/1771752656-Tyd1PzkqO0gsWVH9L8Z42xQo.png?width=1200)

2/7

![画像](https://assets.st-note.com/img/1771752669-pDUOr35PLdSlt9QHGYXRTuCB.png?width=1200)

3/7

![画像](https://assets.st-note.com/img/1771752683-kGsqQnTvtZ97pKMyil16FdJ0.png?width=1200)

4/7

![画像](https://assets.st-note.com/img/1771752698-MzlE6ywWTKI5HjUgm87oaDkC.png?width=1200)

5/7

![画像](https://assets.st-note.com/img/1771752713-HDnGu0WFrZVkvXm3CJsaghbe.png?width=1200)

6/7

![画像](https://assets.st-note.com/img/1771752724-bIHPCZa6m4YvsqFDBGXcrloe.png?width=1200)

7/7

### (2) Harvard Business Reviewスタイル

次に、アメリカの有名なマネジメント誌である **「Harvard Business Review」** のスタイルでスライドを作成してもらいました。内容もビジネスリーダー向けになっています。

> **Harvard Business Reviewスタイルで、2026年ミラノ・コルティナ冬季オリンピック金メダリストのアリサ・リュウについての日本語スライドを作成して**

**【Claude Opus 4.6の回答】**

Now let me read the PPTX skill file to create a professional presentation.The slides look polished overall. Let me fix a few minor text overflow issues on slides 1 and 3.All slides look clean and professional now. Let me copy the final file to the output directory.  
HBRスタイルのプレゼンテーション（全8スライド）を作成しました。

構成は以下の通りです:

1. **タイトルスライド** — 「引退から金メダルへの軌跡」
2. **エグゼクティブサマリー** — プロフィールと3つの重要数字（金メダル2個、226.79点、24年ぶり）
3. **キャリア年表** — 2019年〜2026年の非線形キャリアとKey Insight
4. **リーダーシップフレームワーク** — 戦略的離脱・自律的復帰・結果からの解放の3要因
5. **パフォーマンス分析** — 得点推移チャートと引退前/復帰後の比較表
6. **名言スライド** — リュウ本人のインタビュー引用と内発的動機の考察
7. **ビジネスリーダーへの示唆** — サバティカル、自律性、プロセス志向、非線形キャリアの4つの教訓
8. **結論** — 「休息はより高く跳ぶための準備である」

カラーパレットはHBRのクリムゾンレッドを基調に、ダークチャコール＋ゴールドアクセントで仕上げています。

**完成したスライド**

![画像](https://assets.st-note.com/img/1771754200-mX64ORhgsozv3fdDblSukQrU.png?width=1200)

1/8

![画像](https://assets.st-note.com/img/1771754216-mYnkHQxtJNlse4vpF0Xz5TCR.png?width=1200)

2/8

![画像](https://assets.st-note.com/img/1771754231-qDx3f2jy4JQ1vC8tYNLmB6Ow.png?width=1200)

3/8

![画像](https://assets.st-note.com/img/1771754246-StUZFOM7fzdk9o1NA0ihLTYb.png?width=1200)

4/8

![画像](https://assets.st-note.com/img/1771754257-io4H2lXPdwF8AkUB57jWJuZM.png?width=1200)

5/8

![画像](https://assets.st-note.com/img/1771754270-bSnMrvuYzlCXq5dWf8KUy7xh.png?width=1200)

6/8

![画像](https://assets.st-note.com/img/1771754287-wjHoCcxN18AQiLdkTrIu2abG.png?width=1200)

7/8

![画像](https://assets.st-note.com/img/1771754299-DtAWfP85Ry1gaovUks39lXQB.png?width=1200)

8/8