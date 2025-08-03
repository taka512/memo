# 標準ドキュメント構成 構築プロンプト

## 目的
プロジェクトのドキュメントを、人間とAIの両方にとって分かりやすく、メンテナンス性の高い標準構成に再構築します。特に、[Diátaxisフレームワーク](https://diataxis.fr/)の考え方を全面的に採用し、ドキュメントをその目的に応じて明確に分類します。

## 指示
以下の手順に従って、プロジェクトのドキュメントを **日本語で** 再構築してください。

---

### フェーズ1: 現状分析と情報整理

1.  **既存ドキュメントの分析**:
    現在の `README.md` や、その他存在するすべてのドキュメントを読み込み、内容を完全に把握してください。

2.  **情報の分類 (Diátaxis)**:
    分析した内容を、Diátaxisフレームワークの4つのカテゴリに分類・整理してください。
    -   **チュートリアル (Tutorials)**: プロジェクトのセットアップ手順など、学習者を手引きするレッスン。
    -   **ハウツーガイド (How-to guides)**: コマンド実行、デバッグ、リリース手順など、特定の目標を達成するための具体的な手順。
    -   **解説 (Explanation)**: アーキテクチャ、設計思想、技術選定の理由、過去の経緯など、背景や文脈を理解するための説明。
    -   **リファレンス (Reference)**: API仕様、重要なログ、技術的に正確で網羅的な情報。

---

### フェーズ2: 新しいドキュメント構造の構築

1.  **知識ベース用ディレクトリの作成**:
    プロジェクトのルートに `auto-generated` という名前のディレクトリを作成し、その中にDiátaxisのカテゴリに対応するサブディレクトリを作成します。
    ```bash
    mkdir -p auto-generated/tutorials auto-generated/how-to-guides auto-generated/explanation auto-generated/reference
    ```

2.  **知識ベースの作成**:
    分類した情報を基に、対応するディレクトリ内にファイルを日本語で作成・記述してください。
    -   `auto-generated/how-to-guides/cookbook.md`: **ハウツーガイド**を集約します。
    -   `auto-generated/explanation/knowledge.md`: **解説**（アーキテクチャや設計思想）を集約します。
    -   `auto-generated/explanation/history.md`: **解説**（プロジェクトの歴史と変遷）を集約します。
    -   `auto-generated/reference/debug-log.md`: **リファレンス**（重要なデバッグログなど）を集約します。

3.  **マスタードキュメントの作成 (`CLAUDE.md`)**:
    -   プロジェクトのルートに `CLAUDE.md` というファイル名で、日本語で作成します。
    -   このファイルには、プロジェクト概要と、Diátaxisフレームワークに基づいた新しいナレッジベースへの案内を記述します。以下のテンプレートを参考にしてください。
        ```markdown
        ## ナレッジベース (Diátaxisフレームワーク準拠)

        このプロジェクトの知識は、Diátaxisフレームワークに基づき、`auto-generated/` ディレクトリ内で目的別に分類・管理されています。

        - **[解説 (Explanation)](./auto-generated/explanation/)**: なぜそうなっているのか、背景や設計思想を理解するためのドキュメントです。
        - **[ハウツーガイド (How-to guides)](./auto-generated/how-to-guides/)**: 特定のタスクを達成するための具体的な手順を示します。
        - **[リファレンス (Reference)](./auto-generated/reference/)**: 技術的に正確で網羅的な情報です。
        - **チュートリアル (Tutorials)**: この役割は主に `README.md` が担っています。
        ```
    -   プロジェクトのコンテキストやアーキテクチャの概要もここに含めます。

4.  **人間向け `README.md` の再構築**:
    -   `README.md` を、Diátaxisにおける**チュートリアル**として再定義します。
    -   内容は、プロジェクトのセットアップと基本的な操作に限定し、シンプルに保ちます。
    -   冒頭に、より詳細な情報への入口として `CLAUDE.md` へのリンクを目立つように配置します。

---

### フェーズ3: 仕上げ

1.  **ドキュメント構成図の作成と追記**:
    -   以下のDiátaxisベースのMermaid図を、「ドキュメント構成」セクションとして `README.md` と `CLAUDE.md` の両方の末尾に追記します。
        ```mermaid
        ---
        title: ドキュメント構成図 (Diátaxis準拠)
        ---
        graph TD
            subgraph "アクター"
                direction LR
                A["開発者 (人間)"]
                B["Claude"]
                C["Gemini"]
                D["GitHub Copilot"]
            end

            subgraph "入口となるドキュメント"
                direction LR
                README("README.md<br>[チュートリアル]")
                CLAUDE_MD("CLAUDE.md<br>[マスター文書]")
                GEMINI_MD("GEMINI.md")
                COPILOT_MD(".github/copilot-instructions.md")
            end

            subgraph "知識ベース (auto-generated/)"
                direction TB
                subgraph "解説 (Explanation)"
                    KNOWLEDGE("knowledge.md")
                    HISTORY("history.md")
                end
                subgraph "ハウツー (How-to)"
                    COOKBOOK("cookbook.md")
                end
                subgraph "リファレンス (Reference)"
                    DEBUG("debug-log.md")
                end
            end

            A -- "参照" --> README
            README -- "詳細はこちら" --> CLAUDE_MD
            B -- "参照" --> CLAUDE_MD
            C -- "参照" --> GEMINI_MD
            D -- "参照" --> COPILOT_MD

            GEMINI_MD -.-> |シンボリックリンク| CLAUDE_MD
            COPILOT_MD -.-> |シンボリックリンク| CLAUDE_MD

            CLAUDE_MD -- "参照" --> KNOWLEDGE & HISTORY & COOKBOOK & DEBUG

            style README fill:#cde4ff,stroke:#333,stroke-width:2px
            style CLAUDE_MD fill:#e1f7d5,stroke:#333,stroke-width:2px
        ```

2.  **最終確認**:
    -   作成した新しいファイル構造と、更新された `README.md` および `CLAUDE.md` の内容を提示し、最終的な承認を求めてください。
