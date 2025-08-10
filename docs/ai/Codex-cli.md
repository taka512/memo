OpenAIのCLIインタフェースのAIエージェント

## 料金プラン

https://openai.com/ja-JP/chatgpt/pricing/

```
Free: Explore how AI can help with everyday tasks $0
Plus: Level up productivity and creativity with expanded access $20
Pro: Get the best of OpenAI with the highest level of access $200
```

## インストール

```
npm install -g @openai/codex
```


## 認証

```
codex login
```
　※PRO plan以上でFreeはAPIのTOKENをOPENAI_API_KEYでexportする必要がある

## スラッシュコマンド

```
/init  create an AGENTS.md file with instructions for Codex
現在のプロジェクトに対するAGENTS.mdテンプレートを生成

/status  show current session configuration and token usage
現在のセッション設定やトークン使用状況を表示

/new  start a new chat during a conversation
新しいチャットスレッドを開始し現在の対話履歴や一時メモリをリセット

/mode
動作モードを切り替え。Codex CLIには後述する**「提案モード」「自動編集モード」「全自動モード」**の3形態があり、/modeコマンドで対話中にこれらを変更

/compact  summarize conversation to prevent hitting the context limit            コンテキストの要約（圧縮）。対話履歴が長くなりモデルのコンテキスト上限に近づいた際、現在までの会話を要約して改めて会話を続けるためのコマンドです。
Codex CLIは提案文や履歴を逐次保持しますが、長くなるとトークン制限に抵触するため、このコマンドで過去を短い要約に置き換えてコンテキストウィンドウを節約できます。             


/diff  show git diff (including untracked files)


/prompts  show example prompts


/logout  log out of Codex
/quit  exit Codex
```


## AGENTS.md

Cluade CodeのCLAUDE.mdに相当する指示ファイル


## 設定ファイル 

「~/.codex/config.toml」に記載

```
# approval mode
approval_policy = "untrusted"
sandbox_mode    = "read-only"

# full-auto mode
approval_policy = "on-request"
sandbox_mode    = "workspace-write"

# Optional: allow network in workspace-write mode
[sandbox_workspace_write]
network_access = true
```

## 実行

読み取り/書き込みで実行

```
codex --sandbox workspace-write --ask-for-approval on-request

Codex は、承認なしにワークスペース内でコマンドを実行したり、ファイルに書き込んだりできます。
他のフォルダへのファイルの書き込み、ネットワークへのアクセス、git の更新、その他サンドボックスで保護された操作を実行するには、Codex に許可が必要
デフォルトでは、ワークスペースにはカレントディレクトリと /tmp などの一時ディレクトリが含まれます。ワークスペース内のディレクトリは、/status コマンドで確認可能
```

読み取り専用で実行

```
codex --sandbox read-only --ask-for-approval on-request

Codex は、承認なしに読み取り専用コマンドを実行できます。
ファイルの編集、ネットワークへのアクセス、その他サンドボックスで保護された操作を実行するには、Codex に許可が必要です。
バージョン管理されていないフォルダの推奨デフォルトです。
```

質問なしで実行

```
codex --ask-for-approval never --sandbox read-only

同じワークスペースで複数のエージェントを並行して実行し、質問に回答させる場合
```
GPT-5で実行
```
codex --model gpt-5 --full-auto "Build a simple photobooth application with camera access in a single HTML file"
```

## link

- [OpenAI developer platform](https://platform.openai.com/docs/overview)
- [CodeX(Github)](https://github.com/openai/codex)

