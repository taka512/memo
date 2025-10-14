---
title: "Next.jsのtutorialをやるDockerの環境が欲しい"
source: "https://qiita.com/ryo_one/items/b50bc5a4c9aa25a0943a"
author:
  - "[[ryo_one]]"
published: 2024-07-21
created: 2025-10-15
description: "やりたいこと これを始めようと思ったけど、とにかく自分のパソコン（以下ホストマシン）でnodeやらnpm i などやら実行するのが嫌なので、Dockerでコンテナを立てるまでの記録です。 めちゃくちゃ簡易に作成したものなので色々不足はありますが最低限の構築として。 ..."
tags:
  - "clippings"
image: "https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-user-contents.imgix.net%2Fhttps%253A%252F%252Fcdn.qiita.com%252Fassets%252Fpublic%252Farticle-ogp-background-afbab5eb44e0b055cce1258705637a91.png%3Fixlib%3Drb-4.0.0%26w%3D1200%26blend64%3DaHR0cHM6Ly9xaWl0YS11c2VyLXByb2ZpbGUtaW1hZ2VzLmltZ2l4Lm5ldC9odHRwcyUzQSUyRiUyRnMzLWFwLW5vcnRoZWFzdC0xLmFtYXpvbmF3cy5jb20lMkZxaWl0YS1pbWFnZS1zdG9yZSUyRjAlMkY1MDU3NjglMkZkNTVkYjkwYjIyNjNhZWI5NjBjZDA2Yzg1MmQ2NDU4MjMwZjQ3MGY3JTJGeF9sYXJnZS5wbmclM0YxNTk4MDk4MDg4P2l4bGliPXJiLTQuMC4wJmFyPTElM0ExJmZpdD1jcm9wJm1hc2s9ZWxsaXBzZSZiZz1GRkZGRkYmZm09cG5nMzImcz1mOTBiOGFiYjZmODVmNDIyODJiMWRiOWM0YjUxZWVhOA%26blend-x%3D120%26blend-y%3D467%26blend-w%3D82%26blend-h%3D82%26blend-mode%3Dnormal%26s%3D8515521012e530e94b341d1c0f9a6296?ixlib=rb-4.0.0&w=1200&fm=jpg&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTk2MCZoPTMyNCZ0eHQ9TmV4dC5qcyVFMyU4MSVBRXR1dG9yaWFsJUUzJTgyJTkyJUUzJTgyJTg0JUUzJTgyJThCRG9ja2VyJUUzJTgxJUFFJUU3JTkyJUIwJUU1JUEyJTgzJUUzJTgxJThDJUU2JUFDJUIyJUUzJTgxJTk3JUUzJTgxJTg0JnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnR4dC1jb2xvcj0lMjMxRTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LXBhZD0wJnM9NzIzZTJkZWEwMTlmM2FlMGI0YzRmODY0MDhjZThmNTA&mark-x=120&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTgzOCZoPTU4JnR4dD0lNDByeW9fb25lJnR4dC1jb2xvcj0lMjMxRTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9MzYmdHh0LXBhZD0wJnM9OWU4MzJiOGQ2ZTVhOGM4ZDdjN2E0N2RjMzkzN2JiZTg&blend-x=242&blend-y=480&blend-w=838&blend-h=46&blend-fit=crop&blend-crop=left%2Cbottom&blend-mode=normal&s=062a0ea8d7e401ab1cba12a1ef77d739"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

## Qiita Conference 2025 Autumn

![](https://cdn.qiita.com/assets/public/client-resources/image-karaage0703-971edc712fc1bce7-971edc712fc1bce7.png)

からあげ

生成AI時代のテックブログの始め方

[特設サイトで詳細をチェック](https://qiita.com/official-campaigns/conference/2025-autumn)

この記事は最終更新日から1年以上が経過しています。

[@ryo\_one (リョウイチ)](https://qiita.com/ryo_one)

## Next.jsのtutorialをやるDockerの環境が欲しい

投稿日

## やりたいこと

これを始めようと思ったけど、とにかく自分のパソコン（以下ホストマシン）でnodeやらnpm i などやら実行するのが嫌なので、Dockerでコンテナを立てるまでの記録です。  
めちゃくちゃ簡易に作成したものなので色々不足はありますが最低限の構築として。

## 目標

Getting Startedの以下のコマンドをコンテナ内で実行する

```text
npx create-next-app@latest nextjs-dashboard --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example" --use-pnpm
```

## 手順

### 構成の準備

```text
nextjs-dashboard
├── Dockerfile
└── docker-compose.yml
```

Dockerfile

```dockerfile
FROM node:18.17.1

WORKDIR /app
```

docker-compose.yaml

```yaml
version: '3'
services:
  app:
    build:
      context: .
    volumes:
      - .:/app
    tty: true
    ports:
      - "3000:3000"
```

このtty: true が肝なんですよね。今回run devとかしないでコンテナ内で色々やりたいので、これを書かないと起動した瞬間終了してしまいます。  
初めて使うけど、本来の用途なのかは不明です。とりあえず「コンテナ内で仮想ターミナルを有効にして、コンテナが終了しないようにする設定」という認識でいいと思います。

### コマンド

こちらの記事を参考にさせていただきました。

```text
docker-compose build
docker-compose run --rm app sh
```

これは「一時的にコンテナを起動して、shシェルを実行し、終了後にそのコンテナを自動的に削除するよ」という事です。  
シェルに入ると思うので、その中で以下実行します。

```text
npm install -g pnpm
npx create-next-app@latest nextjs-dashboard --example "https://github.com/vercel/next-learn/tree/main/dashboard/starter-example" --use-pnpm
```

以上で完了です。

## まとめ

ホストマシンに何も入れたくない自分みたいな人間は割とよくやる事になりそうな手順でしたので、備忘録までに投稿しました。  
tty: true くんはこの後どうなるのでしょうか。そして彼の行方を知る者はどこにも居ませんでした。

[0](https://qiita.com/ryo_one/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fryo_one%2Fitems%2Fb50bc5a4c9aa25a0943a&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fryo_one%2Fitems%2Fb50bc5a4c9aa25a0943a&realm=qiita)

## Qiita Conference 2025 Autumn 11月5日(水)~7日(金)開催！

![](https://cdn.qiita.com/assets/public/official_campaigns/qiita_conference_2025_autumn/image-conference_2025_autumn_ogp-7cf3021de31b9ab76a7b3bbaf2909bb5.png)

Qiita Conferenceは、AI時代のエンジニアに贈るQiita最大規模のテックカンファレンスです！

基調講演ゲスト(敬称略)

piacere、牛尾 剛、Esteban Suarez、和田 卓人、seya、ミノ駆動、市谷 聡啓、からあげ、岩瀬 義昌、まつもとゆきひろ、みのるん and more…

[イベント詳細を見る](https://qiita.com/official-campaigns/conference/2025-autumn)

[0](https://qiita.com/ryo_one/items/b50bc5a4c9aa25a0943a/likers)

いいねしたユーザー一覧へ移動

0