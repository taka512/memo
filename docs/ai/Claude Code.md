
## 料金

https://claude.ai/upgrade

```
Free: $0
Pro: $20
Max: $100
Max: $200
```

## install

npm install -g @anthropic-ai/claude-code


## command option

```
# Claude Codeの起動
claude

# 特定のディレクトリで起動
claude /path/to/project

# 最新版への更新
claude update

# 前回の会話を継続
claude -c

# 会話履歴を参照して起動
claude -r

# バージョン確認
claude --version

# 一回限りのタスクを実行
claude "fix the build error"

# 一回限りのクエリを実行して終了
claude -p "explain this function"

# Gitコミットを作成
claude commit

```
## slash command

```
コマンドで効率化
claudeコマンドを実行すると、Claude Codeが起動し、プロンプトを入力できるようになりますが、先頭に/をつけると、Claude Codeの便利ツールが使えます。

# ディレクトリ初期化
/init


# モデルの切り替え（Sonnet/Opus）
> /model

# コンテキストに別フォルダを追加
> /add-dir <path>

# 会話履歴をクリア
> /clear

# コマンド確認
/help

# 今読み込んでいるメモリーを確認
/memory

fix error
```

## Sub agents

- [Claude CodeのSub agentsでコンテキスト枯渇問題をサクッと解決できたはなし](https://zenn.dev/tacoms/articles/552140c84aaefa)
- [Claude Code Sub Agents 実践ガイド：自動委任機能の効果的な活用法！](https://zenn.dev/asuene/articles/d05c8b70da8365)
- [Claude Code でカスタムサブエージェントを作成する](https://azukiazusa.dev/blog/create-custom-sub-agent-in-claude-code/)

## SuperClaude

- [Claude Codeに専門家チームを追加！開発効率を向上させるSuperClaudeを試してみた](https://dev.classmethod.jp/articles/claude-code-superclaude/)

## link


- [Claude Code を初めて使う人向けの実践ガイド](https://zenn.dev/hokuto_tech/articles/86d1edb33da61a)
- [Claude Code ベストプラクティス](https://zenn.dev/farstep/articles/claude-code-best-practices)
- [Claude Codeを徹底解説してみた（前編）](https://dev.classmethod.jp/articles/get-started-claude-code-1/)
- [Claude Codeを徹底解説してみた（後編）](https://dev.classmethod.jp/articles/get-started-claude-code-2nd/)
- [Claude Codeに保守しやすいコードを書いてもらうための事前準備やガードレール](https://www.memory-lovers.blog/entry/2025/06/12/074355)
- [Claude Code Actions を試した！！ && Devin のようにSlackから話しかけたい！！！](https://www.ikkitang1211.site/entry/2025/05/27/232943)
- [Claude Codeをサンドボックス上で実行する(Mac編)](https://zenn.dev/todesking/articles/claude-code-with-sandbox-exec)
- [# Claude Code × HCP Terraformでセキュアなインフラ自動構築を実現する](https://dev.classmethod.jp/articles/claude-code-hcp-tf-authentication-information-management/)