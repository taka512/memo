メモツールのObsidianのあれこれ
## Obsidian Bases

obsidian Basesとは既存のObsidianノートをまとめてデータベースのように扱う事ができるプラグイン by  [Obsidian Bases 入門](https://note.com/shotovim/n/n20d31913131b)
テーブル形式とかカード形式で表示してくれる

## Web Clipper

obsidianに開いているWebページの概要をブックマーク保存する by [Obsidianに記事メモをためる Web Clipper](https://zenn.dev/sh11235/articles/07bb24f98b93e7)
OPTION + SHIFT + Oでブラウザからブックマーク可能

設定からインポートする設定の例
![[docs/default-clipper.json]]

## Obsidian MCP

obsidianにMCP経由でLLMからアクセス可能にする

設定 -> コミュニティプラグインで「MCP tools」「Local REST API」をインストール
ちなみにプラグインはvalue設定ごとにインストールが必要となる

設定 -> コミュニティプラグイン「MCP tools」を選択してMCPサーバーをインストール
ClaudeDesktopであれば「claude_desktop_config.json」に以下の設定を行う
```
stakahashi$ cat  ~/Library/Application\ Support/Claude/claude_desktop_config.json
{
  "mcpServers": {
    "obsidian-mcp": {
      "command": ".obsidian/plugins/mcp-tools/bin/mcp-server",
      "env": {
        "OBSIDIAN_API_KEY": "xxxx"
      }
    },
```
※ OBSIDIAN_API_KEYの値はLocal REST APIから取得
