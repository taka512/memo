

## kiroの設定


- ワークスペース
	- 現在のプロジェクト／ワークスペース固有
	- .kiro/settings/mcp.json に保存
- ユーザー
	- 全プロジェクト／ワークスペースに適用
	- ホームディレクトリ（~/.kiro/settings/mcp.json）に保存


## ドキュメント

- requirements.md
	- EARS（Easy Approach to Requirements Syntax）形式での要件定義
- design.md
	- 技術スタックとアーキテクチャの記述 方式レベルの設計資料
- tasks.md
	- 実装からデプロイまでの詳細な手順 方式設計書をもとに実際に構築を進めるための作業手順書

## MCP

docker MCPへの接続設定

```
cat ~/.kiro/settings/mcp.json
{
  "mcpServers": {
    "MCP_DOCKER": {
      "type": "stdio",
      "command": "docker",
      "args": ["mcp", "gateway", "run"],
      "env": {}
    }
  }
}
```



## link

- [AWS Kiro のインストールと MCP サーバーへの接続（Docker MCP Toolkit）完全ガイド](https://note.com/create_ash/n/nf10348b8f3b6)
- [インフラ屋さんはAIコーディングエージェントとどう生きるか ~ Kiroを使ったWebシステムなアーキテクチャ構築をしてハマった話](https://tech.nri-net.com/entry/building_a_web_system_architecture_using_kiro)
- [Kiro と Model Context Protocol (MCP) で開発生産性を解き放つ](https://aws.amazon.com/jp/blogs/news/unlock-your-development-productivity-with-kiro-and-mcp/)

