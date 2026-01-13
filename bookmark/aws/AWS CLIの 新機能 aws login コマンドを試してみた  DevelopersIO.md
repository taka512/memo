---
title: "AWS CLIの 新機能 aws login コマンドを試してみた | DevelopersIO"
source: "https://dev.classmethod.jp/articles/aws-cli-aws-login/"
author:
  - "[[suzuki.ryo]]"
published: 2025-11-20
created: 2026-01-05
description: "AWS CLIの新コマンド「aws login」がv2.32.0以降で利用可能に。ブラウザベースの認証により、アクセスキーを保存せずセキュアにCLIを利用できます。MFA必須環境でもセキュア、簡単な認証が可能になりました。"
tags:
  - "clippings"
image: "https://images.ctfassets.net/ct0aopd36mqt/wp-thumbnail-afb8c010d7789109015110e5802383b4/506a5c1ec4d5d675831f8f65783b5d2f/aws-cli"
---
2025年11月19日、AWS CLIの認証をブラウザベースで実現する新コマンド **「aws login」** がリリース、AWSマネジメントコンソールにログイン済みのブラウザで承認するだけで、ローカルのAWS CLIが利用できるようになりました。

「開発用端末にアクセスキーを保存したくない」、「MFA必須のIAMユーザーで簡単にCLIを使いたい」という課題を解決する、「aws login」を試す機会がありましたので、紹介します。

## AWS CLIの更新

aws login コマンドは、 AWS CLI version 2.32.0 でサポートされた新機能です。 まずは手元の環境を最新版にアップデートしました。

今回、先日リリースされた Kiro-CLIに依頼して、AWS CLIを version 2.32.1 に更新しました。

```
> macOS、ローカルにインストール済みの AWS CLI v2を、 version 2.32.0 以降の最新バージョンにアップデートして

> macOSでAWS CLI v2をアップデートします。
I will run the following command: curl "https://awscli.amazonaws.com/AWSCLIV2.pkg" -o "/tmp/AWSCLIV2.pkg" && sudo installer -pkg /tmp/AWSCLIV2.pkg -target / (using tool: shell)
Purpose: Download and install latest AWS CLI v2

<略>

I will run the following command: aws --version (using tool: shell)
Purpose: Verify updated AWS CLI version

aws-cli/2.32.1 Python/3.13.9 Darwin/25.1.0 exec-env/AmazonQ-For-CLI Version/1.20.0 exe/arm64
 - Completed in 8.414s

> アップデート完了しました！

AWS CLIのバージョンが 2.27.45 から 2.32.1 にアップデートされました。
 ▸ Time: 50s
```

## aws login を試してみた

`aws login` 操作を実行しました。

環境変数やconfigファイルでデフォルトリージョン等が設定されていない場合、初回のみリージョンの指定を求められます。

```
No AWS region has been configured. The AWS region is the geographic location of your AWS resources.

If you have used AWS before and already have resources in your account, specify which region they were created in. If you have not created resources in your account before, you can pick the region closest to you: https://docs.aws.amazon.com/global-infrastructure/latest/regions/aws-regions.html.

You are able to change the region in the CLI at any time with the command "aws configure set region NEW_REGION".
```

ここでは東京リージョン (ap-northeast-1) を指定しました。

```
AWS Region [us-east-1]: ap-northeast-1
```

OSのデフォルトブラウザが起動します。 （もしブラウザが開かない場合は、コンソールに表示されるURLを手動で開くよう案内が表示されます）

```
If the browser does not open, open the following URL:

https://ap-northeast-1.signin.aws.amazon.com/v1/authorize?response_type=code&client_id=arn%3Aaws%3Asignin%3A%3A%3Adevtools%2Fsame-device&state=<STATE_STRING>&code_challenge_method=SHA-256&scope=openid&redirect_uri=http%3A%2F%2F127.0.0.1%3A<PORT>%2Foauth%2Fcallback&code_challenge=<CODE_CHALLENGE>
```

ブラウザ側では、すでにAWSマネジメントコンソールにログイン済みであれば、そのセッション（IAMユーザーやスイッチロール先）を選択して利用可能です。

![AWSサインイン](https://devio2024-media.developers.io/image/upload/v1763604486/2025/11/20/bbiadrpyhbas4c9i8dqr.jpg)

「新しいセッションにサインイン」で、任意のAWSアカウントにログインしなおす事も可能です。

![新しいセッションにサインイン](https://devio2024-media.developers.io/image/upload/v1763604473/2025/11/20/eguns1nwnd0vq5gqfs5p.jpg)

ブラウザ上で認証に成功すると、以下の画面が表示されます。

![サインイン成功](https://devio2024-media.developers.io/image/upload/v1763604636/2025/11/20/xmx0qpf1hnpwqdz82a5s.jpg)

ターミナルに戻ると、プロファイルの設定が完了した旨が表示されました。

```
Updated profile default to use arn:aws:sts::<AWS_ACCOUNT_ID>:assumed-role/<ROLE_NAME>/<SESSION_NAME> credentials.
```

## 動作確認

`sts get-caller-identity` コマンドを実行し、正しく認証されているか確認します。

```bash
aws sts get-caller-identity

{
    "UserId": "AROAXXXXXXXXXXXXXXXXX:my-session-name",
    "Account": "123456789012",
    "Arn": "arn:aws:sts::123456789012:assumed-role/my-role-name/my-session-name"
}
```

ブラウザでログイン中のロール権限でCLIが実行できる事が確認できました。

## まとめ

aws login の登場により、長期的なアクセスキーを発行することなく、セキュアかつ手軽にCLIが利用できるようになりました。

特に、普段ブラウザでスイッチロールをして複数のアカウントを管理している場合や、Jumpアカウント経由で作業をしている場合など、ブラウザの認証情報をそのままCLIに持ち込めるのは非常に便利です。

これまで aws configure でアクセスキー・シークレットキーを端末に保存して運用していた方は、ぜひ新しい「aws login」への切り替えを検討してみてください。

この記事をシェアする