---
title: "TerraformでAmazon ECSを構築するサンプルコードを書いてみた"
source: "https://qiita.com/chacco38/items/ca9a4362097457373324"
author:
  - "[[chacco38]]"
published: 2023-12-20
created: 2025-10-15
description: "はじめに みなさん、こんにちは。今回はTerraformの入門ということでAmazon ECSのサンプルコードを書いてみましたのでこちらを紹介していきたいと思います。 なお、サンプルコードを書いた際のTerraformおよびAWSプロバイダーのバージョンは次のとおりです。..."
tags:
  - "clippings"
image: "https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-user-contents.imgix.net%2Fhttps%253A%252F%252Fcdn.qiita.com%252Fassets%252Fpublic%252Fadvent-calendar-ogp-background-7940cd1c8db80a7ec40711d90f43539e.jpg%3Fixlib%3Drb-4.0.0%26w%3D1200%26blend64%3DaHR0cHM6Ly9xaWl0YS11c2VyLXByb2ZpbGUtaW1hZ2VzLmltZ2l4Lm5ldC9odHRwcyUzQSUyRiUyRnMzLWFwLW5vcnRoZWFzdC0xLmFtYXpvbmF3cy5jb20lMkZxaWl0YS1pbWFnZS1zdG9yZSUyRjAlMkY2MTM5MDglMkZjMzM1NGFhMGNhYzg2NTM1NDEwNWFlYjQxYWQ3YmI4ZGUxODA0NjVmJTJGeF9sYXJnZS5wbmclM0YxNTg2MDE1NDgzP2l4bGliPXJiLTQuMC4wJmFyPTElM0ExJmZpdD1jcm9wJm1hc2s9ZWxsaXBzZSZiZz1GRkZGRkYmZm09cG5nMzImcz0yZWI5NGM4ZTZmNzkwNzRkNWQyN2NjNzFiNTU3YWFmMA%26blend-x%3D120%26blend-y%3D462%26blend-w%3D90%26blend-h%3D90%26blend-mode%3Dnormal%26mark64%3DaHR0cHM6Ly9xaWl0YS1vcmdhbml6YXRpb24taW1hZ2VzLmltZ2l4Lm5ldC9odHRwcyUzQSUyRiUyRnMzLWFwLW5vcnRoZWFzdC0xLmFtYXpvbmF3cy5jb20lMkZxaWl0YS1vcmdhbml6YXRpb24taW1hZ2UlMkYwOTNmYzlhY2MwNGE1ZDg2NzBjZWExMGIzNGU0NmQ1MWVjNDExYjEzJTJGb3JpZ2luYWwuanBnJTNGMTYzNDcwODQ5Nz9peGxpYj1yYi00LjAuMCZ3PTQ0Jmg9NDQmZml0PWNyb3AmbWFzaz1jb3JuZXJzJmNvcm5lci1yYWRpdXM9OCZiZz1GRkZGRkYmYm9yZGVyPTIlMkNGRkZGRkYmZm09cG5nMzImcz0zMGUxODVhOWNkYzY3ZTQ3Mjc1ZmM1OThiM2MwNzkzNQ%26mark-x%3D186%26mark-y%3D515%26mark-w%3D40%26mark-h%3D40%26s%3D95b84b35e5bf4cf6333212880af86b09?ixlib=rb-4.0.0&w=1200&fm=jpg&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTk2MCZoPTMyNCZ0eHQ9VGVycmFmb3JtJUUzJTgxJUE3QW1hem9uJTIwRUNTJUUzJTgyJTkyJUU2JUE3JThCJUU3JUFGJTg5JUUzJTgxJTk5JUUzJTgyJThCJUUzJTgyJUI1JUUzJTgzJUIzJUUzJTgzJTk3JUUzJTgzJUFCJUUzJTgyJUIzJUUzJTgzJUJDJUUzJTgzJTg5JUUzJTgyJTkyJUU2JTlCJUI4JUUzJTgxJTg0JUUzJTgxJUE2JUUzJTgxJUJGJUUzJTgxJTlGJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnR4dC1jb2xvcj0lMjMzQTNDM0MmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LXBhZD0wJnM9NTM4MDg1N2M1OGFkNzc4Zjk3ZTAxZjcwMWIxYjZhYzE&mark-x=120&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTgzOCZoPTU4JnR4dD0lNDBjaGFjY28zOCZ0eHQtY29sb3I9JTIzM0EzQzNDJnR4dC1mb250PUhpcmFnaW5vJTIwU2FucyUyMFc2JnR4dC1zaXplPTM2JnR4dC1wYWQ9MCZzPWVjY2Y2ZDI1MmI5NDI5ODJkMTA1YmI2NGQzYTc0NmRh&blend-x=242&blend-y=454&blend-w=838&blend-h=46&blend-fit=crop&blend-crop=left%2Cbottom&blend-mode=normal&txt64=5qCq5byP5Lya56S-5pel56uL6KO95L2c5omAIC8g44Kv44Op44Km44OJ44Ko44Oz44K444OL44Ki44Oq44Oz44Kw44OB44O844Og&txt-x=242&txt-y=539&txt-width=838&txt-clip=end%2Cellipsis&txt-color=%233A3C3C&txt-font=Hiragino%20Sans%20W6&txt-size=28&s=a44f569a595349f85a5fb2ca0de492e0"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

## Qiita Conference 2025 Autumn

![](https://cdn.qiita.com/assets/public/client-resources/image-yukihiro_matz-62ee5a1ddc705baf-62ee5a1ddc705baf.png)

まつもとゆきひろ

ベテランによるAI時代のプログラミング（2025年秋版）

[特設サイトで詳細をチェック](https://qiita.com/official-campaigns/conference/2025-autumn)

この記事は最終更新日から1年以上が経過しています。

## はじめに

みなさん、こんにちは。今回はTerraformの入門ということでAmazon ECSのサンプルコードを書いてみましたのでこちらを紹介していきたいと思います。

なお、サンプルコードを書いた際のTerraformおよびAWSプロバイダーのバージョンは次のとおりです。最新バージョンでは定義方法が異なっている可能性があるため、実際にコードを書く際は最新の「 [Terraformドキュメント](https://developer.hashicorp.com/terraform/docs) 」と「 [AWSプロバイダードキュメント](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) 」を確認しながら開発を進めていただければと思います。

**例）versions.tf**

versions.tf

```tf
# Requirements
terraform {
  required_version = "~> 1.3.6"

  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.46.0"
    }
  }
}
```

## サンプルコードを書いてみた

今回は次のような構成のサンプルコードを書いてみました。なお、変数定義部分などの一部省略している点、ならびにステップごとの細かい説明などは省いていますのでご承知おきください。詳細については「 [AWSプロバイダードキュメント](https://registry.terraform.io/providers/hashicorp/aws/latest/docs) 」をご参照ください。

- [例1. ECSクラスタの作成](https://qiita.com/chacco38/items/#%E4%BE%8B1-ecs%E3%82%AF%E3%83%A9%E3%82%B9%E3%82%BF%E3%81%AE%E4%BD%9C%E6%88%90)
- [例2. タスク定義の作成](https://qiita.com/chacco38/items/#%E4%BE%8B2-%E3%82%BF%E3%82%B9%E3%82%AF%E5%AE%9A%E7%BE%A9%E3%81%AE%E4%BD%9C%E6%88%90)
- [例3. ECSサービスの作成](https://qiita.com/chacco38/items/#%E4%BE%8B3-ecs%E3%82%B5%E3%83%BC%E3%83%93%E3%82%B9%E3%81%AE%E4%BD%9C%E6%88%90)

ちなみに、すべてのサンプルコードに共通してプロバイダー定義は次のようにしています。

**例）providers.tf**

providers.tf

```tf
# デフォルトのプロバイダー設定
provider "aws" {
  region = var.aws_region
  default_tags {
    tags = {
      Owner     = "matt"
      Terraform = "true"
    }
  }
}
```

## 例1. ECSクラスタの作成

ECSクラスタの例でContainer Insightsを有効化している以外には特筆するところはないかと思います。

**例）main.tf**

main.tf

```tf
# ECSクラスタの作成
resource "aws_ecs_cluster" "this" {
  name = var.ecs_cluster_name

  setting {
    name  = "containerInsights"
    value = "enabled"
  }

  tags = {
    Name = var.ecs_cluster_name
  }
}
```

## 例2. タスク定義の作成

AWS Fargate向けのタスク定義の例で、タスク起動用IAMロールやコンテナ用IAMロールなども併せて作成しています。

**例）main.tf**

main.tf

```tf
# タスク定義の作成
resource "aws_ecs_task_definition" "this" {
  family                   = var.ecs_task_name
  network_mode             = "awsvpc"
  requires_compatibilities = ["FARGATE"]
  cpu                      = var.ecs_task_cpu
  memory                   = var.ecs_task_memory
  execution_role_arn       = aws_iam_role.ecs_execution.arn
  task_role_arn            = aws_iam_role.ecs_task.arn

  container_definitions = jsonencode([
    # 割愛
  ])

  tags = {
    Name = var.ecs_task_name
  }
}

# タスク起動用IAMロールの定義
resource "aws_iam_role" "ecs_execution" {
  name = var.iam_ecs_execution_role_name

  assume_role_policy = jsonencode({
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "",
        "Effect": "Allow",
        "Principal": {
          "Service": "ecs-tasks.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
      }
    ]
  })

  tags = {
    Name = var.iam_ecs_execution_role_name
  }
}

# タスク起動用IAMロールへのポリシー割り当て
resource "aws_iam_role_policy_attachment" "ecs_execution" {
  policy_arn = "arn:aws:iam::aws:policy/service-role/AmazonECSTaskExecutionRolePolicy"
  role       = aws_iam_role.ecs_execution.name
}

# コンテナ用IAMロールの定義
resource "aws_iam_role" "ecs_task" {
  name = var.iam_ecs_task_role_name

  assume_role_policy = jsonencode({
    "Version": "2012-10-17",
    "Statement": [
      {
        "Sid": "",
        "Effect": "Allow",
        "Principal": {
          "Service": "ecs-tasks.amazonaws.com"
        },
        "Action": "sts:AssumeRole"
      }
    ]
  })

  tags = {
    Name = var.iam_ecs_task_role_name
  }
}

# コンテナ用IAMポリシーの定義（例ではSSMパラメータストアのアクセス権限を付与）
resource "aws_iam_policy" "ecs_task" {
  name = var.iam_ecs_task_policy_name
  path = "/service-role/"

  policy = jsonencode({
    "Version": "2012-10-17",
    "Statement": [
      {
        "Action": [
          "ssm:GetParametersByPath",
          "ssm:GetParameters",
          "ssm:GetParameter"
        ],
        "Effect": "Allow",
        "Resource": [
          "*"
        ]
      }
    ]
  })

  tags = {
    Name = var.iam_ecs_task_policy_name
  }
}

# コンテナ用IAMロールへのポリシー割り当て
resource "aws_iam_role_policy_attachment" "ecs_task" {
  policy_arn = aws_iam_policy.ecs_task.arn
  role       = aws_iam_role.ecs_task.name
}
```

## 例3. ECSサービスの作成

AWS Fargate向けのECSサービスの例で、タスク起動用IAMロールやコンテナ用IAMロールなども併せて作成しています。

**例）main.tf**

main.tf

```tf
# ECSサービスの作成
resource "aws_ecs_service" "this" {
  name                              = var.ecs_service_name
  cluster                           = aws_ecs_cluster.this.id
  task_definition                   = aws_ecs_task_definition.this.arn
  desired_count                     = var.ecs_desired_count
  health_check_grace_period_seconds = var.ecs_health_check_grace_period_seconds
  launch_type                       = "FARGATE"
  force_new_deployment              = var.ecs_force_new_deployment

  network_configuration {
    security_groups = [aws_security_group.this.id]
    subnets         = [for x in data.aws_subnet.private : x.id]
  }

  load_balancer {
    target_group_arn = aws_lb_target_group.backend.arn
    container_name   = var.ecs_container_name
    container_port   = var.ecs_container_port
  }

  tags = {
    Name = var.ecs_service_name
  }

  depends_on = [aws_lb.cc_external_nlb]
}

# セキュリティグループの定義（通信制御の定義は割愛）
resource "aws_security_group" "this" {
  name   = var.security_group_name
  vpc_id = data.aws_vpc.this.id

  tags = {
    Name = var.security_group_name
  }

  lifecycle {
    create_before_destroy = true
  }
}

# NLB向けのECSサービス用ターゲットグループの定義
resource "aws_lb_target_group" "this" {
  name        = var.nlb_target_group_name
  port        = var.ecs_container_port
  protocol    = "TCP"
  target_type = "ip"
  vpc_id      = data.aws_vpc.this.id

  tags = {
    Name = var.elb_target_group_name
  }
}
```

**例）data.tf**

data.tf

```tf
# VPC情報の取得
data "aws_vpc" "this" {
  cidr_block = var.vpc_cidr_block
}

# サブネット情報の取得
data "aws_subnet" "private" {
  for_each = var.private_subnets

  vpc_id            = data.aws_vpc.default.id
  availability_zone = each.value.availability_zone
  cidr_block        = each.value.cidr_block
}
```

**例）outputs.tf**

outputs.tf

```tf
output "security_group_id" {
  description = "The ID of the security group"
  value       = try(aws_security_group.this.id, "")
}
```

## 終わりに

今回はTerraformの入門ということで、Amazon ECSのサンプルコードをいくつかご紹介してきましたがいかがだったでしょうか。こんな記事でも誰かの役に立っていただけるのであれば幸いです。

なお、今回ご紹介したコードはあくまでサンプルであり、動作を保証するものではございません。そのまま使用したことによって発生したトラブルなどについては一切責任を負うことはできませんのでご注意ください。

---

- AWS は、米国その他の諸国における Amazon.com, Inc. またはその関連会社の商標です。
- Terraform は、HashiCorp, Inc. の米国およびその他の国における商標または登録商標です。
- その他、記載されている会社名および商品・製品・サービス名は、各社の商標または登録商標です。

[0](https://qiita.com/chacco38/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fchacco38%2Fitems%2Fca9a4362097457373324&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fchacco38%2Fitems%2Fca9a4362097457373324&realm=qiita)

## Qiita Conference 2025 Autumn 11月5日(水)~7日(金)開催！

![](https://cdn.qiita.com/assets/public/official_campaigns/qiita_conference_2025_autumn/image-conference_2025_autumn_ogp-7cf3021de31b9ab76a7b3bbaf2909bb5.png)

Qiita Conferenceは、AI時代のエンジニアに贈るQiita最大規模のテックカンファレンスです！

基調講演ゲスト(敬称略)

piacere、牛尾 剛、Esteban Suarez、和田 卓人、seya、ミノ駆動、市谷 聡啓、からあげ、岩瀬 義昌、まつもとゆきひろ、みのるん and more…

[イベント詳細を見る](https://qiita.com/official-campaigns/conference/2025-autumn)

[2](https://qiita.com/chacco38/items/ca9a4362097457373324/likers)

いいねしたユーザー一覧へ移動

4