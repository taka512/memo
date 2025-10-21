---
title: "MCPの認証と認可の現在と未来 | hi120ki"
source: "https://hi120ki.github.io/ja/blog/posts/20250728/"
author:
  - "[[hi120ki]]"
published: 2025-07-28
created: 2025-10-21
description: "MCPの認証と認可の現在と未来"
tags:
  - "clippings"
image: "https://hi120ki.github.io/ja/img/2025-07-28/ogp.png"
---
[English](https://hi120ki.github.io/blog/posts/20250728/)

2025年7月現在、MCPは2024-11-05、2025-03-26、2025-06-18の3つのバージョンを経て進化し、私達は手元のMCPクライアントとなるCursorやClaude CodeやVSCodeからnpxコマンドやuvコマンドやDockerコンテナによってMCPサーバーを起動したりRemote MCPサーバーに接続するなど日常的に使うようになりました。

実際に私自身がよく利用しているMCPライブラリである [MCP Servers - Cursor](https://docs.cursor.com/tools/mcp) を見てみると多くのサービスが独自にMCPサーバーの提供に投資している様子が伺え、エコシステムは拡大していっています。

![Cursor MCP Servers](https://hi120ki.github.io/ja/assets/images/cursor-mcp-servers-b71c33e1f4d1bf4de639baaf014f561e.png)

例: Notion、Figma、Linear、GitHub

特に [Atlassian Remote MCP Server beta](https://community.atlassian.com/forums/Atlassian-Platform-articles/Using-the-Atlassian-Remote-MCP-Server-beta/ba-p/3005104) はサービスが公式に提供するMCPサーバーの中で、最も先進的なものの一つです。

Cursorを立ち上げると自動的にブラウザが立ち上がりAtlassianアカウントへのログインへ誘導され、認可画面で許可するアクセス権限の範囲を確認し、その後MCPサーバーを経由してAI Agentが私自身のConfluenceやJiraリソースにアクセスできるようになります。

このOAuthフローはエンドユーザーにとって非常に便利です。API Keyを発行したり、有効期限や保管方法を気にする必要はありません。短命アクセストークンの発行はMCP仕様側の推奨であり、具体的な寿命や更新挙動はAS実装（Atlassian等）のポリシーによりますが、有効期限が切れれば再度ブラウザが誘導してくれます。

このOAuthフローはMCPが既存OAuthエコシステムと整合しやすい設計（OAuth 2.1準拠、AS分離、PKCE等の原則）を提供することで実現されているのですが、ただ一方でこのMCPのOAuth仕様は利用拡大を妨げている欠点もあります。この記事ではその欠点を解説しつつ、MCP仕様変更の試みとOktaが取ろうとしている解決アプローチを公開されている資料から推察し紹介します。これにより、Enterprise環境でのMCPの利用拡大のロードマップ策定の助けになれば幸いです。

> 私はOAuthの専門家ではないため、不正確な内容が含まれる可能性があります。詳細は [References](https://hi120ki.github.io/ja/blog/posts/20250728/#references) に挙げた資料をご覧ください。

## Dynamic Client Registrationの欠点

MCPのOAuth仕様は基本的にことが知られています。

つまり、以下のような4つの登場人物が存在します。

- **Client**: MCPクライアント、CursorやClaude Code、MCP Inspectorなど
- **Resource Server**: MCPサーバーおよびMCPサーバーがアクセスする保護されたリソース(Confluence、Jiraなど)を持つサーバー
- **Authorization Server**: OAuthにおける認可サーバー、サービスのアクセストークンを発行するサーバー
- **Browser**: ユーザーが操作するブラウザ

があります。つまりクライアントは認可サーバーに登録され、ブラウザを介して認可サーバーからアクセストークンを取得し、リソースサーバーにアクセスするという流れです。

一般的にOAuth 2.1では、クライアントは事前に認可サーバーに登録されている必要があります。例えばConfidential Clientはアクセストークンを取得するためのClient ID(とClient Secret)を持つことができます。

しかし、実際にMCPを利用する際には、以下の設定でOAuthフローを実行することができます。Client ID(とClient Secret)は事前に設定されており、認可サーバーに登録されている必要があるはずですが、この設定では省略されています。

```json
"mcp-atlassian": {
  "url": "https://mcp.atlassian.com/v1/sse"
}
```

この理由としてはMCP仕様ではクライアントが事前に認可サーバーに登録されていることは必須ではありません。MCPはを利用することを推奨しており、クライアントは事前の登録なしに `/register` エンドポイントを通じて認可サーバーに登録され、アクセストークンを取得することができます。

これは非常に便利な機能であり、簡単にMCPを利用するために設計されています。しかし、このDynamic Client Registrationにはいくつかの欠点があります。

1. **セキュリティの懸念**: Dynamic Client Registrationは、クライアントが認可サーバーに登録される際に、Client ID(とClient Secret)を自動的に生成しますが、クライアントが不正に登録される可能性があります。特に、エンタープライズ環境では、セキュリティポリシーにより、事前に登録されたクライアントのみが許可されることが一般的です。
2. **Dynamic Client Registrationのサポートが必要**: Dynamic Client Registrationを利用するためには、認可サーバーがこの機能をサポートしている必要がありますが、すべての認可サーバーがこの機能をサポートしているわけではありません。特にエンタープライズ環境では、既存の認可サーバーがDynamic Client Registrationをサポートしていない場合があります。

OktaはDynamic Client Registration自体はサポートしますが、管理者権限に基づく認証(例：APIトークン)を要するため、無認証のDynamic Client Registrationは不可能です。Googleについては公開文書上、汎用のDynamic Client Registration APIは提供されておらず、一般にコンソールでの静的クライアント登録が前提です。つまり、OktaやGoogleを直接MCPにおけるOAuthの認可サーバーとして配置することは不可能です。

ではAtlassianのRemote MCP Serverはどのように追加のクレデンシャルなしにDynamic Client Registrationをサポートしているのでしょうか。彼らは独自にDynamic Client Registrationを実装したAuthorization Serverを提供しています。

AtlassianはOAuth 2.1ベースのASメタデータを公開しており、Remote MCP Serverの接続開始時にブラウザ経由のOAuthフローへ誘導されます([https://mcp.atlassian.com/v1/sse](https://mcp.atlassian.com/v1/sse))が、ここでAtlassianはMCP専用の認可サーバーを提供し、そこからさらに本来のAtlassianアカウントのエコシステムを構成している認可サーバーを利用しています。

この2つの認可サーバーを組み合わせて利用する方式は現在よく取り入れられており、もし自身で試したい場合は [remote-mcp-github-oauth - Cloudflare AI](https://github.com/cloudflare/ai/tree/main/demos/remote-mcp-github-oauth) でその詳細を学ぶことができます。 [workers-oauth-provider](https://github.com/cloudflare/workers-oauth-provider) というCloudflareが提供しているOAuth providerを構成するためのライブラリを利用し、独自にDynamic Client Registrationを構成し提供することができます。

しかしこの方式は実装が複雑となり、また同時にMCPサーバーを構成する人が認可サーバーの責任を追う必要があることを意味します。一般的にOAuthのような仕組みは高度な知識とスキルがないと脆弱性を含めてしまうリスクがあり、自前での実装やメンテナンスは避けるべきとされています。ただし、Cloudflare Workersのworkers-oauth-providerのような実績あるライブラリを活用すれば、.well-known/DCR/PKCEまで下支えできるため、選定と運用体制次第でリスク低減は可能です。それでも一般的な企業の中で内部の従業員向けにMCPサーバーのアクセス制限を正しく構成したいという動機に対しては、このアプローチは依然としてリスクとコストが大きすぎると判断されるでしょう。

## 不明瞭なBackend Servicesへの認可の仕様

一般的な企業の中で内部の従業員向けにMCPサーバー、とくにこれらのMCPサーバーが社内のリソース(Backend Services)にアクセスするときのベストプラクティスは何でしょうか?

社内のリソースとはAmazonやGoogle CloudやAzureといったクラウドのデータベース、New RelicやData DogといったObservability Platformからのメトリクス・ログ、そしてGoogle Workspace・Notion・Confluenceといったナレッジベースなどです。これらとMCPサーバーを連携し自動化を行うことは喫緊の課題です。とはいいつも、これらのリソースについてMCPサーバーに"安全な"方法でアクセス権限を付与することは困難です。

一般的に用いられる手法はMachine UserやService Accountで統一された権限でアクセス権限を付与することです。これは非常にセットアップが容易です。一つの権限のみに着目し、それをある一定のアクセス制限の元で公開するのみです。しかしこれは [Confused deputy problem](https://docs.aws.amazon.com/IAM/latest/UserGuide/confused-deputy.html) を引き起こす場合があります。ある組織ではナレッジベースに特定の部門(HRやFinance部門など秘匿性の高い部署)のみが閲覧できるドキュメントがあるにもかかわらす、Machine Userで統一された権限でナレッジベースの全体ドキュメント読み取り権限がMCPサーバーに対して構成され、そのMCPサーバーが全従業員がアクセスできるような設定で公開されたとしましょう。別の部門に所属するある従業員は本来与えられたアカウントの権限では閲覧が禁止されていたはずの重要な社内ドキュメントに社内MCPサーバーを介してアクセスすることができるようになってしまいます。これはMCPやAIに限らず起きる問題ではありますが、既存の部門ごとや個人ごとに設定されたアクセス境界を破壊する重大な構成ミスとなります。

この場合にベストプラクティスとなるアプローチはユーザーごとにOAuthフローを介してユーザーの既存のアクセス権限のサブセットであることが保証されたアクセストークンを取得し、それのみを使ってMCPサーバーがBackend Servicesのリソースへアクセスします。これによって既存のアクセス境界が守られConfused deputy problemは発生しません。

MCP本体は [RFC 9728(Protected Resource Metadata)](https://datatracker.ietf.org/doc/html/rfc9728) と [RFC 8707(Resource Indicators)](https://datatracker.ietf.org/doc/html/rfc8707) で"どのASからどうトークンを得るか"の土台を整備しました。RFC 9728により、クライアントは保護リソースのメタデータから正しいAuthorization Server・リソース識別子を動的に発見でき、RFC 8707によって適切なスコープとリソースを指示してトークン取得できます。これらはMCPのAuthorization Server分離設計と整合し、複数Authorization Serverを横断した認可フローの基盤となっています。一方で現時点では複数のBackend Servicesからリソースを取得しつつ一定の機能を提供するようなMCPサーバーを適切な認可の仕組みとともに提供するベストプラクティスが未確立であり、Enterprise環境において従業員向けに提供される集中管理されたMCPサーバー群を提供するときの壁となります。

> これらの制限を回避する方法のアイデアの一つは、認可が完了していない段階では `auth` ToolのみをMCPサーバーから提供し、このToolからOAuthフローを開始するURLをMCPクライアントに提供しアクセストークンを取得した後 [List Changed Notification](https://modelcontextprotocol.io/specification/2025-06-18/server/tools#list-changed-notification) を送信し、本来利用可能であったToolをMCPクライアントに公開するというものです。
> 
> しかしあくまでこれは既存のMCP仕様を利用した代替案であり実際にベストプラクティスとなるかには疑いが残ります。

このようにDynamic Client Registrationにおける既存エコシステムの限られたサポート状況、そしてBackend ServicesへのOAuthによる適切なアクセス権限付与において現在のMCP仕様は残念ながらエンタープライズ環境で十分に成熟しているとは言えません。

しかし一方で現在MCP仕様のレポジトリにはこれらの改善を期待できるPRがあります。それが [Enterprise-Managed Authorization Profile for MCP #646](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/646) です()。

これは現在Internet-Draftとして公開されている [Identity Assertion Authorization Grant](https://datatracker.ietf.org/doc/draft-parecki-oauth-identity-assertion-authz-grant/) をベースとして構成された新たなフローです。

ここでは5つの登場人物が存在します。

- **Browser**: ユーザーが操作するブラウザ
- **MCP Client**: MCPクライアント、CursorやClaude Code、MCP Inspectorなど
- **Identity Provider**: OktaやGoogleなどのEnterprise環境でのユーザー管理プロバイダー
- **MCP Authorization Server**: AtlassianやNotionやData Dogが提供する認可サーバー
- **MCP Resource Server**: MCPサーバーおよびMCPサーバーがアクセスする保護されたリソース(Confluence、Jiraなど)を持つサーバー

は以下のようになっています。

1. ユーザーがMCPサーバーの利用をCursor等のMCPクライアントで有効にすると、自動的にブラウザが開かれIdP(OktaやGoogle等)のログイン画面が表示され、そのまま従業員ID等を用いてログインします。
2. IdPでのログイン成功後、認可コードがブラウザを経由してMCPクライアントに提供されMCPクライアントはその認可コードを使って、IdPからIdentity Assertionの仕組みのためのIDトークンを取得します。このID TokenはMCPクライアントに保存されます。
3. MCPクライアントはID TokenとアクセスしようとしているMCPサーバーの識別子を含めて、IdPからIdentity Assertion JWT Authorization Grant(ID-JAG)の仕組みを使ってAssertion Token(Identity Assertion JWT)を取得します。
4. MCPクライアントはID-JAGの仕組みを使ってAssertion Tokenを送信し、アクセストークンを取得します。
5. MCPクライアントはこのアクセストークンを使ってMCP Resource Serverにアクセスします。

MCPクライアントがID Tokenを取得した後のプロセスはブラウザを一切介せず行われるということになり、ユーザーの視点から見るとIdPでのログインが完了すると自動的にIdP管理者から許可されている範囲のサービスのアクセストークンが自動的に取得されるようになります。サービスごとにアクセストークンを発行し設定したりOAuthフローを実行する必要はなくなり、IdPでのログインという1つのアクションでMCPサーバーが提供する機能へアクセスできるようになります。

## Oktaが掲げるCross App Accessとは

このIdentity Assertion Authorization GrantをベースとしたMCPのOAuth仕様においてOktaのようなIdPはどのような役割を果たすのでしょうか? これらはOktaが提供している [いくつかのCross App Accessに関する公開情報](https://www.okta.com/newsroom/press-releases/okta-introduces-cross-app-access-to-help-secure-ai-agents-in-the/) から推察できます。

このフローの大きな特徴の一つはIdPが各サービスのアクセストークン発行のハブとなることです。

Enterprise環境のIdP管理者は一般的にOktaやGoogle上でユーザーを管理するだけでなくサービス自体も管理しています。実際にそれぞれのEnterprise環境ごとにOktaやGoogleで多くのサービスが統合されSCIMによる自動プロビジョニングとSAMLによるIdP経由のログインという体験をしていると思います。それらに加えてこの仕様ではユーザー管理・サービス管理だけではなくアクセストークンの管理もIdPに委任できるようになることを意味します。IdP管理者はどのOAuth Clientからどのサービスのアクセストークンを自動的に取得できるようになるのかを管理できるようになるほか、アクセストークンの発行状況もID-JAGの発行状況から可視性を得ることができます。

今まではそれぞれのサービス内で独自に行われていたアクセストークン発行のプロセスにIdPが入り込むことにより、Enterprise環境の管理者がより中央集権的な管理と可視性を得ることができるようになり、各サービス上で今まで見過ごされてきていた長期間有効なアクセストークンやAPIキーを置き換えることができるようになります。

## 現時点の懸念点

まず懸念点として挙げられるのが各サービスの対応状況です。基本的にこのような"ユーザーになりすますことができ、かつ指定されたスコープでのアクセストークンをユーザーからの承認アクションなく発行できる機能"は著名なものだとGoogle WorkspaceのDomain Wide Delegationが挙げられますが、まだ未対応のサービスがほとんどだと思われます。Okta自体のCross App Access機能については、2025年第3四半期（Q3）に選定顧客向けの機能提供予定と公表されており（2025年6月23日リリース）、ISV/エンタープライズの両面でエコシステムの足並みが鍵となります。各サービスの対応状況も継続してウォッチしていく必要があります。

また、MCPでの利用について [Enterprise-Managed Authorization Profile for MCP #646](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/646) ではOAuthクライアントの登録について以下のようにDynamic Client Registrationは用いず、MCPクライアントはIdPおよびMCP Authorization Serverに登録されている、つまりOAuthにおけるClient ID(やClient Secret)に当たるものを事前にユーザーに配布する必要があることを指摘しています。

> The MCP client will likely need to be pre-registered with the enterprise IdP for single sign-on.
> 
> It is also assumed that the MCP client will be pre-registered with the MCP server’s authorization server.

基本的にEnterprise環境ではMDM等のユーザー環境への一定のコントロールが存在しますが、Cursor・Claude Code・MCP Inspectorなどの多岐にわたるMCPクライアントにおいてこの事前登録を具体的にどう進めるのかは示されていません。つまり、私達はIdP・MCP Authorization ServerだけでなくそれぞれのMCP Clientのサポート開始を待つ必要があります。

## さいごに

MCPでの現時点での認証と認可の仕様は残念ながらEnterprise環境では活用が難しいものとなっていますが一方でそれらを改善する動きがあり、さらに既存のEnterprise環境内のアクセス権限管理をよりIdPで集中管理できるように発展していく未来があります。

管理の集中化・可視性の向上・ユーザー体験の向上は歓迎するものであるものの、まだ超えなければならない壁があるのもわかっているためこれらの動向を引き続き追っていきたいと思います。

(更新:2025/9/5) [MCP Meetup Tokyo 2025](https://aiau.connpass.com/event/365588/) において追加の最新情報を加えとして発表を行いました。

## References

- [Enterprise-Ready MCP - Aaron Parecki](https://aaronparecki.com/2025/05/12/27/enterprise-ready-mcp)
- [Enterprise-Managed Authorization Profile for MCP #646](https://github.com/modelcontextprotocol/modelcontextprotocol/pull/646)
- [Identity Assertion Authorization Grant - IETF Datatracker](https://datatracker.ietf.org/doc/draft-parecki-oauth-identity-assertion-authz-grant/)
- [oktadev/id-assertion-authz-node-example](https://github.com/oktadev/id-assertion-authz-node-example)
- [Cross-App Access - Okta Platform](https://www.okta.com/integrations/cross-app-access/)
- [Okta introduces Cross App Access to help secure AI agents in the enterprise](https://www.okta.com/newsroom/press-releases/okta-introduces-cross-app-access-to-help-secure-ai-agents-in-the/)