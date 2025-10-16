
[コーディングエージェントの能力を拡張する Serena を試してみた](https://azukiazusa.dev/blog/serena-coding-agent/)を参考に設定

serenaをローカルで起動

```
uvx --from git+https://github.com/oraios/serena serena start-mcp-server
```

claude codeにMCP設定を追加

```
claude mcp add serena -s project -- uvx --from git+https://github.com/oraios/serena serena start-mcp-server --context ide-assistant --project $(pwd)
```

claude desktopにMCP設定を追加

```
{
  "mcpServers": {
        "serena": {
            "type": "stdio",
            "command": "uvx",
            "args": [
                "--from",
                "git+https://github.com/oraios/serena",
                "serena",
                "start-mcp-server",
                "--context",
                "ide-assistant",
                "--project",
                "/Users/xxx/sapper-blog-app"
            ],
            "env": {}
        },
    }
}
```


## プロジェクトで最初にやる事

オンボーディング

```
「Serena のオンボーディングを開始してください」プロンプトに入力
オンボーディング完了後に「/clear」
```


プロジェクトの設定ファイル

```
.serena/project.yml
```

