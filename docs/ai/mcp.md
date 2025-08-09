
MCPはPCに直接設定する方法をDocker MCP Gatewayを利用してDocker経由でMCPを利用する方法がある
ローカル環境はなるべく汚したくないのでDocker MCP Gatewayを採用

![[MCP-Gateway-blog-Fig.avif]]

## Docker MCP Gateway

インストール

```
## リリース一覧から最新バージョンを取得
## https://github.com/docker/mcp-gateway/releases
$ wget https://github.com/docker/mcp-gateway/releases/download/v0.13.0/docker-mcp-linux-amd64.tar.gz
$ tar xvfz docker-mcp-linux-amd64.tar.gz
$ mkdir -p ~/.docker/cli-plugins 
$ mv docker-mcp ~/.docker/cli-plugins 
$ chmod +x ~/.docker/cli-plugins/docker-mcp
```

MCPの設定はダッシュボードから有効化するのが良さそう（認証もそこでできる）

![[mcp.docker.png]]

カタログ一覧
```
docker mcp catalog show
```
MCP有効化
```
docker mcp server enable fetch
```
MCP無効化
```
docker mcp server disable slack
```
一覧
```
docker mcp server list
```

## MCP server

[MCP Servers for agent mode](https://code.visualstudio.com/mcp)

利用するMCP server

- Playwright MCP
	- ブラウザテストを行う。スクショ撮ってくれる
-  GitHub MCP Server
	- githubの操作
- Fetch MCP Server
	- Webページを取得して、その内容をMarkdownに変換する
- terraform
	- terraformの知見を授けてくれるっぽい
- AWS Documentation
	- AWSの知見を授けてくれる
-  AWS Terraform
	- AWS terraformでのbest practiceを授けてくれる

## link

- [MCPとは何か 〜AIエージェントの為の標準プロトコル〜](https://blog.cloudnative.co.jp/27994/)
- [Model Context Protocol（MCP）とは？生成 AI の可能性を広げる新しい標準](https://zenn.dev/cloud_ace/articles/model-context-protocol)
- [Obsidian MCPサーバーをClaude Desktopで使ってみた](https://dev.classmethod.jp/articles/obsidian-mcp-claude-desktop-integration-hands-on/)
- [o3 MCPでClaude Codeが最強の検索力を手に入れた](https://zenn.dev/yoshiko/articles/claude-code-with-o3)
- [Docker MCP Gatewayがすんばらしい](https://qiita.com/moritalous/items/8789a37b7db451cc1dba)
- [Docker MCP Gateway日本語](https://www.docker.com/ja-jp/blog/docker-mcp-gateway-secure-infrastructure-for-agentic-ai/)
- [Docker MCP Gateway公式](https://docs.docker.com/ai/mcp-gateway/)

