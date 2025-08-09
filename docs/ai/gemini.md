
## install

```
sudo npm install -g @google/gemini-cli
```

## 実行

インタラティブモードで実行

```
gemini
```

コマンドプロンプトから実行

```
gemini --prompt "1から10までの素数をリストアップして" 
```

## 書式

接頭辞
```
- `@<ファイルパス>` または `@<ディレクトリパス>`  
    指定されたファイルまたはディレクトリの内容を読み込み、プロンプトのコンテキストに含めます。`.gitignore` を尊重するため、不要なファイル（`node_modules` など）は自動的に除外されます。  
    例: `@src/utils.ts この関数のテストを書いてください。`
    
- `!<シェルコマンド>`  
    指定されたコマンドを直接シェルで実行し、結果を表示します。  
    例: `!git diff --staged`
    
- `!` (単体)  
    シェルモードのオン/オフを切り替えます。シェルモード中は、すべての入力がシェルコマンドとして扱われます。
```

スラッシュコマンド

```
/quit または /exit
Gemini CLIを終了します。

/clear
ターミナルの表示をクリアします。（ショートカット: Ctrl+L）

/chat
会話の履歴を管理します。

/chat save <タグ名>: 現在の会話に名前（タグ）を付けて保存します。
/chat resume <タグ名>: 保存した会話を呼び出します。
/chat list: 保存した会話のタグリストを表示します。
/memory
GEMINI.md ファイルによって読み込まれたAIの記憶（コンテキスト）を管理します。

/memory show: 現在AIが記憶しているコンテキストの全内容を表示します。
/memory refresh: GEMINI.md ファイルをすべて再読み込みします。
/memory add <記憶させたいテキスト>: 現在のセッションに限り、一時的な記憶を追加します。
/tools
現在利用可能なツールの一覧を表示します。

/tools desc: 各ツールの詳細な説明を表示します。
/mcp
接続されているMCP（Model Context Protocol）サーバーと、そこから提供されるツールの一覧を表示します。

/restore [tool_call_id]
--checkpointing が有効な場合、ツール実行前の状態にプロジェクトファイルを復元します。IDを指定しない場合は、復元可能なチェックポイントの一覧を表示します。

/compress
それまでの会話履歴をAIに要約させ、その要約でコンテキストを置き換えます。トークンを節約したい長い会話の途中で便利です。

/stats
現在のセッションにおけるトークン使用量、API呼び出し回数、時間などの統計情報を表示します。

/theme
CLIの見た目のテーマを選択するダイアログを開きます。

/auth
認証方法を変更するためのダイアログを開きます。

/editor
差分表示などに使用するエディタを選択するダイアログを開きます。

```

## コンテキストファイル

基本はGEMINI.mdだが以下の設定でCLAUDE.mdを優先設定可能
```
.gemini/settings.json
{
  "contextFileName": ["CLAUDE.md","GEMINI.md"]
}
```

## サンドボックスモード

```
サンドボックスモードでの実行(-sをつける)
gemini -s -p "hoge削除"
```

サンドボックスが守ってくれるもの

```
- ファイルシステムの保護

ブロックされる操作: ホームディレクトリ直下の重要な設定ファイル（例: ~/.ssh/, ~/.gitconfig）や、ドキュメントフォルダ（例: ~/Documents/）など、指定された場所以外への書き込みが禁止されます。
許可される操作: 現在作業しているプロジェクトディレクトリ内でのファイル操作や、一時ディレクトリへの書き込みは許可されます。

- ネットワークの制御

意図しない外部への通信を防ぐため、ネットワークアクセスを完全に遮断したり、特定のプロキシ経由の通信のみを許可したりといった制御が可能です。（詳細は後述のプロファイルで解説します）

- プロセスの隔離

サンドボックス内での操作は、ホストOSから隔離された環境で実行されます。これにより、システム全体に影響を及ぼすようなコマンドの実行が制限され、安全性が保たれます。
```

## Gemini at MCP

個別のMCPの設定をする場合

.gemini/settings.json
```
{
  "theme": "Xcode",
  "selectedAuthType": "oauth-personal",
  "mcpServers": {
    "qiita-mcp": {
      "command": "node",
      "args": [
        "/Users/n0bisuke/ds/3_project/mcp/qiita-mcp/fetch-server.js"
      ],
      "env": {
        "QIITA_TOKEN": "XXXXXXXXXXXXXXXXXXX"
      }
    },
  }
}
```

### Docker MCPの追加

Docker MCPを経由する場合は個別のサーバーの設定はせずにDocker MCP serverの設定

```
~/.gemini/settings.json
{
  "mcpServers": {
    "MCP_DOCKER": {
      "command": "docker",
      "args": ["mcp", "gateway", "run"],
      "env": {}
    }
  }
}
```

インストールの確認


```
$ gemini

> /mcp

ℹ Configured MCP servers:
 
  🟢 MCP_DOCKER - Ready (1 tools)
    - fetch
```

## Gemini Code Assist

IDEのコーディングアシスタント

### .gemini/styleguide.md 

リポジトリ内に.gemini/styleguide.mdを作成するとgeminiの動作のカスタマイズが行える
https://developers.google.com/gemini-code-assist/docs/customize-gemini-behavior-github?hl=ja

## link

- [Gemini CLIの全社利用を支える技術](https://techblog.zozo.com/entry/technologies-supporting-company-wide-use-of-gemini-cli)
- [Gemini CLI の簡単チュートリアル](https://zenn.dev/schroneko/articles/gemini-cli-tutorial)
- [Gemini CLI のサンドボックス機能とは](https://tech.algomatic.jp/entry/2025/06/26/055948)