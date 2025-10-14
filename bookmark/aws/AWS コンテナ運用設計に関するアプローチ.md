---
title: "AWS コンテナ運用設計に関するアプローチ"
source: "https://qiita.com/Shmwa2/items/be5caef0407cb4746517"
author:
  - "[[Shmwa2]]"
published: 2022-02-01
created: 2025-10-15
description: "ECSメインにAWSサービスを利用してコンテナの運用設計を考えてみます。 コンテナの運用設計 ECS 上で稼働するWebアプリケーションを前提に運用の要件を考えてみます。 コンテナを使用したマイクロサービスの運用は、モノシリックなシステム運用とは少し異なります、以下の項..."
tags:
  - "clippings"
image: "https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-user-contents.imgix.net%2Fhttps%253A%252F%252Fcdn.qiita.com%252Fassets%252Fpublic%252Farticle-ogp-background-afbab5eb44e0b055cce1258705637a91.png%3Fixlib%3Drb-4.0.0%26w%3D1200%26blend64%3DaHR0cHM6Ly9xaWl0YS11c2VyLXByb2ZpbGUtaW1hZ2VzLmltZ2l4Lm5ldC9odHRwcyUzQSUyRiUyRnFpaXRhLWltYWdlLXN0b3JlLnMzLmFwLW5vcnRoZWFzdC0xLmFtYXpvbmF3cy5jb20lMkYwJTJGMTU5Nzg5OCUyRnByb2ZpbGUtaW1hZ2VzJTJGMTYyNDc3Nzg4OT9peGxpYj1yYi00LjAuMCZhcj0xJTNBMSZmaXQ9Y3JvcCZtYXNrPWVsbGlwc2UmYmc9RkZGRkZGJmZtPXBuZzMyJnM9ODI5OTE2NWFiMmMwZTc1MDVjYTk3YmM5ZGMyNjFkMDQ%26blend-x%3D120%26blend-y%3D462%26blend-w%3D90%26blend-h%3D90%26blend-mode%3Dnormal%26mark64%3DaHR0cHM6Ly9xaWl0YS1vcmdhbml6YXRpb24taW1hZ2VzLmltZ2l4Lm5ldC9odHRwcyUzQSUyRiUyRnMzLWFwLW5vcnRoZWFzdC0xLmFtYXpvbmF3cy5jb20lMkZxaWl0YS1vcmdhbml6YXRpb24taW1hZ2UlMkY3MzYwOWYxZDVhZjAxM2VlNGZjNmExOWExYjQyYTdkYjJkYTA3ODE5JTJGb3JpZ2luYWwuanBnJTNGMTYxNzYxMDgwMD9peGxpYj1yYi00LjAuMCZ3PTQ0Jmg9NDQmZml0PWNyb3AmbWFzaz1jb3JuZXJzJmNvcm5lci1yYWRpdXM9OCZiZz1GRkZGRkYmYm9yZGVyPTIlMkNGRkZGRkYmZm09cG5nMzImcz0wZGFhNTFmNGEyOWM1ZTQwOWYxNDE3YjQ3OTI1ZGNkOQ%26mark-x%3D186%26mark-y%3D515%26mark-w%3D40%26mark-h%3D40%26s%3Da696a933ca6030be4fb6045b273db7be?ixlib=rb-4.0.0&w=1200&fm=jpg&mark64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTk2MCZoPTMyNCZ0eHQ9QVdTJTIwJUUzJTgyJUIzJUUzJTgzJUIzJUUzJTgzJTg2JUUzJTgzJThBJUU5JTgxJThCJUU3JTk0JUE4JUU4JUE4JUFEJUU4JUE4JTg4JUUzJTgxJUFCJUU5JTk2JUEyJUUzJTgxJTk5JUUzJTgyJThCJUUzJTgyJUEyJUUzJTgzJTk3JUUzJTgzJUFEJUUzJTgzJUJDJUUzJTgzJTgxJnR4dC1hbGlnbj1sZWZ0JTJDdG9wJnR4dC1jb2xvcj0lMjMxRTIxMjEmdHh0LWZvbnQ9SGlyYWdpbm8lMjBTYW5zJTIwVzYmdHh0LXNpemU9NTYmdHh0LXBhZD0wJnM9MDRhYzhlZjU3OGNjNjViMjQ2YTY2OWUzODFjMDc1YmM&mark-x=120&mark-y=112&blend64=aHR0cHM6Ly9xaWl0YS11c2VyLWNvbnRlbnRzLmltZ2l4Lm5ldC9-dGV4dD9peGxpYj1yYi00LjAuMCZ3PTgzOCZoPTU4JnR4dD0lNDBTaG13YTImdHh0LWNvbG9yPSUyMzFFMjEyMSZ0eHQtZm9udD1IaXJhZ2lubyUyMFNhbnMlMjBXNiZ0eHQtc2l6ZT0zNiZ0eHQtcGFkPTAmcz1hZjYyOTMwYzMwMWFkMjRmMjM3YzBjY2NmOGYxNjE3ZQ&blend-x=242&blend-y=454&blend-w=838&blend-h=46&blend-fit=crop&blend-crop=left%2Cbottom&blend-mode=normal&txt64=5qCq5byP5Lya56S-QmVlWA&txt-x=242&txt-y=539&txt-width=838&txt-clip=end%2Cellipsis&txt-color=%231E2121&txt-font=Hiragino%20Sans%20W6&txt-size=28&s=ddcea133eaa90bd0ebd40aa821eb6e38"
---
![](https://relay-dsp.ad-m.asia/dmp/sync/bizmatrix?pid=c3ed207b574cf11376&d=x18o8hduaj&uid=)

## Qiita Conference 2025 Autumn

![](https://cdn.qiita.com/assets/public/client-resources/image-ushio-d75bb8c20524325a-d75bb8c20524325a.png)

牛尾 剛

Agent SDK Deep Dive

[特設サイトで詳細をチェック](https://qiita.com/official-campaigns/conference/2025-autumn)

この記事は最終更新日から3年以上が経過しています。

ECSメインにAWSサービスを利用してコンテナの運用設計を考えてみます。

## コンテナの運用設計

ECS 上で稼働するWebアプリケーションを前提に運用の要件を考えてみます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/c17c4cc4-2889-e7f3-a905-5316895f2bd2.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Fc17c4cc4-2889-e7f3-a905-5316895f2bd2.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4aefc32974986efc3d9ba8b498d5f79f)

コンテナを使用したマイクロサービスの運用は、モノシリックなシステム運用とは少し異なります、以下の項目を運用項目としてピックアップします。

- ***可用性/スケーリング***
- ***CI/CD***
- ***ロギング***
- ***トレース***
- ***モニタリング***

## ECS/ECR のアーキテクチャ

まずはECS/ECR のアーキテクチャについて触れます。

***Amazon ECSはコンテナの作成、実行、停止といった管理をメインとしたサービスであり、Amazon ECRは Dockerのレジストリサービスとなります。リポジトリにあるイメージをプッシュしたり、イメージの保管等を行います。***

全体的なイメージを以下のように理解をしています。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/708c69e9-7c24-649e-4dbb-39e74ef9d620.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F708c69e9-7c24-649e-4dbb-39e74ef9d620.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=32851660fb203eeb40187bba929a92c7)

## ECSの機能

まずECSです。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/486bc96b-8bca-8b6a-d92e-5243c15beaec.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F486bc96b-8bca-8b6a-d92e-5243c15beaec.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7c0a7cd45a912338b6cdfbde64ddba14)

***ECSは複数のエンティティから機能を果たすため、各々どのような役割を担うかを理解する必要があるかと思います。以下は包含関係と関係性を図解として描いています。***

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/6c892485-4671-e1c7-75e6-f51d7f15f362.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F6c892485-4671-e1c7-75e6-f51d7f15f362.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=0e408fe1f5a7e38f2574df2672cd3e34)

### Task definition(タスク定義)

`task definition` とは起動する `task` の情報が定義されたもので、  
`docker-compose.yml` のようなイメージです。

起動するタイプ(`EC2` or `Farate`)をタスク定義から定義し、1つのタスク定義上に複数のコンテナを定義する事が可能です。  
task definitionの `image` パラメーター上で、後述の `ECR image` に対してアクセスし、ECR image をそのまま `Task` として実行する事も出来ます。

また、後述する `Service` は `task definition` を指定する必要があります。

サンプルとして `task definition` が保持するデータを表示させてみます。  
イメージ情報やコンピューティングリソース、通信プロトコルなどが定義されている事が判ります。

sample1-describe-task-definition.json

```json
{
    "taskDefinition": {
        "taskDefinitionArn": "arn:aws:ecs:ap-northeast-1:xxxxxx:task-definition/php-sample-fargate:4",
        "containerDefinitions": [
            {
                "name": "php-sample-fargate",
                "image": "xxxx.dkr.ecr.ap-northeast-1.amazonaws.com/test:latest",
                "cpu": 256,
                "memoryReservation": 128,
                "portMappings": [
                    {
                        "containerPort": 80,
                        "hostPort": 80,
                        "protocol": "tcp"
                    }
                ],
                "essential": true,
                "environment": [],
                "mountPoints": [],
                "volumesFrom": [],
                "logConfiguration": {
                    "logDriver": "awslogs",
                    "options": {
                        "awslogs-group": "/ecs/php-sample-fargate",
                        "awslogs-region": "ap-northeast-1",
                        "awslogs-stream-prefix": "ecs"
                    }
                }
            }
        ],
        "family": "php-sample-fargate",
        "taskRoleArn": "arn:aws:iam::xxxxxxx:role/ecsTaskExecutionRole",
        "executionRoleArn": "arn:aws:iam::xxxxxxxx:role/ecsTaskExecutionRole",
        "networkMode": "awsvpc",
        "revision": 4,
        "volumes": [],
        "status": "ACTIVE",
        "requiresAttributes": [
            {
                "name": "com.amazonaws.ecs.capability.logging-driver.awslogs"
            },
            {
                "name": "ecs.capability.execution-role-awslogs"
            },
            {
                "name": "ecs.capability.execution-role-ecr-pull"
            },
            {
                "name": "com.amazonaws.ecs.capability.docker-remote-api.1.18"
            },
            {
                "name": "ecs.capability.task-eni"
            }
        ],
        "placementConstraints": [],
        "compatibilities": [
            "EC2",
            "FARGATE"
        ],
        "requiresCompatibilities": [
            "FARGATE"
        ],
        "cpu": "256",
        "memory": "512",
        "registeredAt": "2021-07-16T10:21:54.039000+09:00",
        "registeredBy": "arn:aws:sts::xxxxxxxxxx:assumed-role/xxxxxxx
    }
}
```

`Task definition` は `JSON` にて定義出来ます。  
なお、AWS CLI を使用してタスク定義のテンプレートを生成する事が可能で、  
空のタスク定義のテンプレートがAWSドキュメントに記載されています。

> タスク定義テンプレート  
> 以下に示しているのは、空のタスク定義テンプレートです。このテンプレートを使用してタスク定義を作成します。これにより、コンソールの JSON 入力領域に貼り付けるか、ファイルに保存して AWS CLI の --cli-input-json オプションで使用できるようになります。詳細については、「タスク定義パラメータ」を参照してください。

### ECS Cluster

クラスター自体は、後述する `ACTIVE` な `Service` 、 `Task` が実行される論理的なグループという理解で良いかと思います。  
また、クラスターを作成する際に、 `VPC/サブネット` などのネットワーク情報を定義します。  
サンプルとして `ECSクラスター` が保持するデータを表示させてみます。

### Service

`Service` は、 `Task definition` で指定された `Task` に紐づき、ECSクラスター上で `ACTIVE` となります。  
主幹は `ロードバランサー` や `AutoScaling` をECSクラスター上で機能させる要素です。

また、後述の `Task` とは機能が分離しており、ECSを立ち上げる必須機能ではありません。

サンプルとして `Service` が保持するデータを表示させてみます。  
`ロードバランサー` の情報や、 `スケーリングポリシー` 、後述のTaskの `リビジョン` を管理、制御する `タスクセット(tasksets)` と呼ばれる情報を返しています。  
なお、 `aws ecs create-service --cli-input-json` により定義されたJSONファイルからサービスの作成も可能です。

sample3-describe-ecs-service.json

```json
{
    "services": [
        {
            "serviceArn": "arn:aws:ecs:ap-northeast-1:xxxxxxxx:service/fargate-cluster/php-samplefargate",
            "serviceName": "php-samplefargate",
            "clusterArn": "arn:aws:ecs:ap-northeast-1:xxxxxxxxx:cluster/fargate-cluster",
            "loadBalancers": [
                {
                    "targetGroupArn": "arn:aws:elasticloadbalancing:ap-northeast-1:xxxxxxxx:targetgroup/tg-fargat-php-samplefargate-2/xxxxxx",
                    "containerName": "test",
                    "containerPort": 80
                }
            ],
            "serviceRegistries": [],
            "status": "ACTIVE",
            "desiredCount": 1,
            "runningCount": 1,
            "pendingCount": 0,
            "launchType": "FARGATE",
            "platformVersion": "1.4.0",
            "taskDefinition": "arn:aws:ecs:ap-northeast-1:xxxxxxxx:task-definition/test-task:1",
            "deploymentConfiguration": {
                "maximumPercent": 200,
                "minimumHealthyPercent": 100
            },
            "taskSets": [
                {
                    "id": "ecs-svc/xxxxxxxxx",
                    "taskSetArn": "arn:aws:ecs:ap-northeast-1:xxxxxxxxx:task-set/fargate-cluster/php-samplefargate/ecs-svc/xxxxxxxxx","clusterArn": "arn:aws:ecs:ap-northeast-1:386755752236:cluster/fargate-cluster",
                    "startedBy": "CodeDeploy",
                    "externalId": "xxxxxxxxx",
                    "status": "PRIMARY",
                    "taskDefinition": "arn:aws:ecs:ap-northeast-1:xxxxxxxxx:task-definition/test-task:1",
                    "computedDesiredCount": 1,
                    "pendingCount": 0,
                    "runningCount": 1,
                    "createdAt": "2021-07-16T11:04:41.383000+09:00",
                    "updatedAt": "2022-01-21T08:54:17.942000+09:00",
                    "launchType": "FARGATE",
                    "platformVersion": "1.4.0",
                    "networkConfiguration": {
                        "awsvpcConfiguration": {
                            "subnets": [
                                "subnet-xxxxxxxxx",
                                "subnet-xxxxxxxxx"
                            ],
                            "securityGroups": [
                                "xxxxxxxxx"
                            ],
                            "assignPublicIp": "DISABLED"
                        }
                    },
                    "loadBalancers": [
                        {
                            "targetGroupArn": "arn:aws:elasticloadbalancing:ap-northeast-1:xxxxxxxxx:targetgroup/tg-fargat-php-xxxxxxxxx",
                            "containerName": "test",
                            "containerPort": 80
                        }
                    ],
                    "serviceRegistries": [],
                    "scale": {
                        "value": 100.0,
                        "unit": "PERCENT"
                    },
                    "stabilityStatus": "STABILIZING",
                    "stabilityStatusAt": "2022-01-21T08:54:17.942000+09:00",
                    "tags": []
                }
            ],
            "deployments": []
...
```

### Task

`タスク定義` からECSクラスター上に起動したコンテナの集合体であり、アプリケーションの処理をメインに行います。  
また、ECSクラスター上で稼働しているTaskは、 `起動/停止` が可能です。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/754d50bb-d597-e413-b641-4f078a1d00e9.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F754d50bb-d597-e413-b641-4f078a1d00e9.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9a912ecc059a12b49e33cf2cb9b216a1)

## ECRの機能

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/f2c19e60-efeb-aa9f-6bfc-fd808da088cd.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Ff2c19e60-efeb-aa9f-6bfc-fd808da088cd.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=85aa5acfc43833930777fe24e23ad4b9)

ECR の主要コンポーネントである `レジストリ` / `リポジトリ` / `イメージ` についてです。

### Registry

レジストリ内にイメージリポジトリを作成し、イメージを保存します。  
レジストリ自体は `認証` (Dockerクライアントとの認証許可を行う事で、レジストリ内のリポジトリとの間でイメージをプッシュ、プルできるようにする)、 `レプリケーション` (AWSのクロスリージョン、クロスアカウントに対するイメージのレプリケーション)等によるものなので、設定情報という理解で良いかと思います。

### Repository

後述の `コンテナイメージ` は `Amazon ECRリポジトリ` に保存されます。  
また、2020年12月以降、ECRはパブリック領域としての利用が可能となり、コンテナイメージをパブリックに公開する事も、ダウンロードすることが可能になりました。

### image

`docker image` のようなもので、イメージをリポジトリ内へ `push(docker push)` して、登録を行います。イメージは `URI形式` で連携が可能で、ECSのタスク定義として使用出来ます。実態は `S3` へ配置されます。

## 稼動システムの全体像

コンテナの運用をするために複数のAWSサービスを使用します。  
今回は、一般的なWebアプリケーションを想定し、以下の構成図から運用を考えていきます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/f0fb773b-31e9-9764-60e5-0bf792a4d624.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Ff0fb773b-31e9-9764-60e5-0bf792a4d624.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ef5b2ef59e94679e57c9374118b90e71)

## Monitoring

ECSに対して監視を行う場合、 `Cloudwatch メトリクス` と `Cloudwatch Container Insights` の二種類の機能を使用する状況を把握する事が出来ます。  
いずれもメトリクスと呼ばれる指標となる `データポイント` を一定期間で集計したもので、対象のメトリクスが閾値を超えた場合、アラームを送信するなどの対策も可能です。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/3d31a885-a7e1-be51-1800-afac608a8693.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F3d31a885-a7e1-be51-1800-afac608a8693.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=a5fa5066be8c3b2dd1842e8b5dbf45cb)

### Cloudwatch メトリクス

Cloudwatch メトリクスは、ECSのサービス単位で `MemoryUtilization` / `CPUUtilization` の２つの指標をモニターします。

| メトリクス | 説明 |
| --- | --- |
| ***MemoryUtilization*** | クラスター、サービス単位で使用されているメモリーの割合(%) |
| ***CPUUtilization*** | クラスター、サービスで使用されている CPU の割合(%) |

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/11efdb09-74ec-7153-421b-0d37455a7b7c.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F11efdb09-74ec-7153-421b-0d37455a7b7c.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=e321ebda5bfc4a3a169f1193e398cbe9)

***`MemoryUtilization` / `CPUUtilization` では、クラスター・サービスいずれもタスクの総数から除算する方式で算出されるため、タスク全てが均衡化された数値としてカウントされるようです。これは、特定のタスクへ負荷をモニタリングする要件にはマッチしない事を意味します。***

### Cloudwatch Container Insights

`Cloudwatch Container Insights` は、 `Cloudwatchメトリクス` より詳細なメトリクスを取得出来ます。また、メトリクス情報を返すリソースの粒度(クラスター、サービス、タスク)も拡張されるためモニタリング機能としてはCloudwatch Container Insightsの方が情報量に優れています。

Amazon ECS クラスターで Container Insights を有効にし、以下のいずれか、または両方を使用する事で多くのメトリクス情報を返します。

- クラスターレベル、タスクレベル、およびサービスレベルのメトリクスの収集を開始するには、 `AWS Management Console` または `AWS CLI` を使用します。
- CloudWatch エージェントを `DaemonSet` としてデプロイし、インスタンスでホストされているクラスターで Amazon EC2 インスタンスレベルのメトリクスの収集を開始します。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/d8fbfc16-914b-2ee1-7189-d2d3c125ddde.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Fd8fbfc16-914b-2ee1-7189-d2d3c125ddde.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ab87a640c83fd7c63d5ea180b155c27c)

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/40880fe8-69d5-1be8-13ac-e3daba701988.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F40880fe8-69d5-1be8-13ac-e3daba701988.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=74ace7cd54a349f3b635c3f0a9116b65)

| メトリクス | 対象 | 説明 |
| --- | --- | --- |
| ***ContainerInstanceCount*** | ClusterName | クラスターに登録されている Amazon ECS エージェントを実行している EC2 インスタンス数 |
| ***CpuUtilized*** | TaskDefinitionFamily、ClusterName、ServiceName | 指定した対象のリソースのタスクにより使用されている CPU ユニット数 |
| ***CpuReserved*** | TaskDefinitionFamily、ClusterName、ServiceName | 指定した対象のリソースのタスクにより予約されている CPU ユニット数 |
| ***DeploymentCount*** | ServiceName、ClusterName | ECS サービスでのデプロイの数 |
| ***DesiredTaskCount*** | ServiceName、ClusterName | ECS サービスに必要なタスクの数 |
| ***MemoryUtilized*** | TaskDefinitionFamily、ClusterName、ServiceName | 指定した対象のリソースのタスクにより使用されているメモリ |
| ***MemoryReserved*** | TaskDefinitionFamily、ClusterName、ServiceName | 指定した対象のリソースのタスクにより予約されているメモリ |
| ***NetworkRxBytes*** | TaskDefinitionFamily、ClusterName、ServiceName | 指定した対象のリソースにより受信されるバイト/秒 ※ネットワークモードがawsvpc、bridgeが対象 |
| ***NetworkTxBytes*** | TaskDefinitionFamily、ClusterName、ServiceName | 指定した対象のリソースにより送信されるバイト/秒 ※ネットワークモードがawsvpc、bridgeが対象 |
| ***PendingTaskCount*** | ServiceName、ClusterName | PENDING 状態にあるタスクの数 |
| ***RunningTaskCount*** | ServiceName、ClusterName | RUNNING 状態にあるタスクの数 |
| ***ServiceCount*** | ClusterName | クラスター内のサービスの数 |
| ***StorageReadBytes*** | TaskDefinitionFamily、ClusterName、ServiceName | 指定した対象のリソースのストレージから読み込まれたバイト数 |
| ***StorageWriteBytes*** | TaskDefinitionFamily、ClusterName、ServiceName | 指定した対象のリソースのストレージヘ書き込まれたバイト数 |
| ***TaskCount*** | ClusterName | クラスターで実行されているタスクの数 |
| ***TaskSetCount*** | ServiceName、ClusterName | サービス内のタスクセットの数 |

### CloudWatch エージェントからEC2インスタンスレベルのメトリクスの収集

| メトリクス | 対象 | 説明 |
| --- | --- | --- |
| ***instance\_cpu\_limit*** | ClusterName | クラスター内の単一の EC2 インスタンスに割り当てることができる CPU ユニットの最大数 |
| ***instance\_cpu\_reserved\_capacity*** | ClusterName、InstanceId、ContainerInstanceId | クラスター内の単一の EC2 インスタンスで現在予約されている CPU ユニットの最大数 |
| ***instance\_cpu\_usage\_total*** | ClusterName | クラスター内の単一 EC2 インスタンスで使用されている CPU ユニットの数 |
| ***instance\_cpu\_utilization*** | ClusterName、InstanceId、ContainerInstanceId | クラスター内の単一の EC2 インスタンスで使用されている CPU ユニットの合計割合 |
| ***instance\_filesystem\_utilization*** | ClusterName、InstanceId、ContainerInstanceId | クラスター内の単一の EC2 インスタンスで使用されているファイルシステム容量の合計割合 |
| ***instance\_memory\_limit*** | ClusterName | このクラスター内の単一の EC2 インスタンスに割り当てることができるメモリの最大量（バイト単位） |
| ***instance\_memory\_reserved\_capacity*** | ClusterName、InstanceId、ContainerInstanceId | クラスター内の単一の EC2 インスタンスで現在予約されているメモリの割合 |
| ***instance\_memory\_utilization*** | ClusterName、InstanceId、ContainerInstanceId | クラスター内の単一の EC2 インスタンスで使用されているメモリの合計割合 |
| ***instance\_memory\_working\_set*** | ClusterName | クラスター内の単一の EC2 インスタンスで使用されているメモリの量（バイト単位） |
| ***instance\_network\_total\_bytes*** | ClusterName | クラスター内の単一の EC2 インスタンスでネットワーク上で送受信された 1 秒あたりの合計バイト数 |
| ***instance\_number\_of\_running\_tasks*** | ClusterName | クラスター内の単一の EC2 インスタンスで実行中のタスクの数 |

CWエージェントは、ECR経由でのエージェントが入ったイメージの導入も可能です。

### ネットワークモードについて

メトリクス `NetworkRxBytes` / `NetworkTxBytes` で出てきたネットワークモードについて少し触れます。  
ECSの `ネットワークモード` とは、タスクのネットワーク動作を定義するものです。  
ネットワークモードは、主に以下3点となりますが、 ***Fargateでのタスクネットワーキングは `awsvpc` のみとなります。***

- ***awsvpc***

タスクには、独自の `Elastic Network Interface (ENI)` とプライマリプライベート IPv4 アドレスが割り当てられます。これにより、タスクに Amazon EC2 インスタンスと同じネットワークプロパティが与えられます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/b7c28ac5-1aec-15de-c44f-28843be12ce4.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Fb7c28ac5-1aec-15de-c44f-28843be12ce4.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3819fa4c2ba987fbf6c6b31c39a6df58)

- ***bridge***

タスクは、タスクをホストする各 Amazon EC2 インスタンス内で実行される `Docker の組み込み仮想ネットワーク` を利用します。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/1347175f-1745-abfe-24c0-89915cf2c83f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F1347175f-1745-abfe-24c0-89915cf2c83f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=79af6863993bd3febb186251869924c5)

- ***host***

タスクは Docker の組み込み仮想ネットワークをバイパスし、タスクをホストしている Amazon EC2 インスタンスの ENI にコンテナポートを直接マッピングします。その結果、 ***ポートマッピングが使用されている場合、1 つの Amazon EC2 インスタンスで同じタスクのインスタンスを複数実行することはできません。***

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/9ce80595-24c8-b8de-1f31-bb758966e7a1.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F9ce80595-24c8-b8de-1f31-bb758966e7a1.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=4c7f3e132a1cf58de2136c7b3f41b126)

## 可用性とScaling

ECSの可用性とScalingはServiceの機能がカバーします。  
それぞれの観点から見ていきます。

## 可用性

ECSのService には `サービススケジューラ(schedulingStrategy)` というパラメーターが存在します。

`schedulingStrategy` では、以下の2つを指定出来ます。

- ***REPLICA***

クラスター全体で必要な数のタスク(DesiredCount)を配置して維持します。デフォルトでは、サービススケジューラによってタスクはアベイラビリティーゾーン間で分散する構成を自動で行います。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/7928cfe6-dd00-60d3-9173-e70e34cde701.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F7928cfe6-dd00-60d3-9173-e70e34cde701.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=34de36587d9e38ae9dfe114e00609777)

- ***DAEMON***

クラスター内のアクティブなコンテナインスタンスごとに、1 つのタスクのみをデプロイします。サービススケジューラは、実行中のタスクのタスク配置制約(後述の参考URL)を評価し、配置制約を満たさないタスクを停止します。  
つまりインスタンス数とタスク数が 1対1で実行するように維持します。  
DAEMONタイプの設定は、 ***Fargate 起動タイプでは利⽤が出来ません。***

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/4f48b840-aea7-0d83-c3ef-228ca71eed01.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F4f48b840-aea7-0d83-c3ef-228ca71eed01.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=30430fbb56cf24c67d59a014aa98da42)

## Scaling

ECS上で機能するスケーリングについて考えていきます。  
スケーリングには、 `スケールアップ` (タスクサイズと呼ばれるtaskのCPU/memoryの増強)と `スケールアウト/イン` が考えられますが、今回はTaskの停止を伴わないスケールアウト/インについて触れていきます。

### AutoScaling

***ECSでは、ServiceへAuto Scalingを設定する事が出来ます。(Service Auto Scaling)***

***これはサービスの必要タスク数(DesiredCount)を⾃動的に増減させるというものです。***

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/86eac0ca-ecbb-f5a3-8464-a9823634b697.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F86eac0ca-ecbb-f5a3-8464-a9823634b697.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=3296e5890bd9aa94ee4a10d9bfcebe1f)

- ***タスクの必要数***  
	Service Auto Scaling で使用するタスクの数
- ***タスクの最小数***  
	Service Auto Scaling で使用するタスクの下限数
- ***タスクの最大数***  
	Service Auto Scaling で使用するタスクの上限数

なお、 `タスクの必要数` は、 `最小タスク数` と `最大タスク数` の範囲内である必要があります。

`Service Auto Scaling` では、オプション機能により、ユーザー定義の条件に従ってスケーラブルする仕組みとなっており、ECSの `CloudWatch メトリクス` を使用して、ピーク時に対処するためにサービスをスケールアウトし (実行するタスクを増やし)、使用率の低い期間にコストを削減するためにサービスをスケールインする (実行するタスクを減らす) ことができます。

以下の2タイプが `Service Auto Scaling` としてサポートしています。

- ***ステップスケーリング***

アラーム違反の大きさに応じて異なる一連のスケーリング調整値に基づいてリソースをスケールします。  
以下２つのメトリクスから選択し、任意のパーセンテージに応じたスケールアウト/インが動作する仕組みです。

- ***ECSServiceAverageCPUUtilization***  
	サービスの平均 CPU 使用率
- ***ECSServiceAveregemorutilization***  
	サービスのメモリ平均使用率

例として、メトリクス `ECSServiceAverageCPUUtilization` を使用し、 `30 - 70%` を通常時のtaskカウントとし、 `30%未満` でtask 1つへスケールイン、 `70 - 89%` でtaskを3つへスケールアウト、 `90%以上` はtaskを4つへスケールアウトなどという設定が可能です。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/d76298bd-83f6-e29d-8ce0-dfb229c86773.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Fd76298bd-83f6-e29d-8ce0-dfb229c86773.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6a54d02aa9eebd6d6a2283d958abed31)

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/6df3779f-56cf-6fb0-ac5c-0be6f8e8674e.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F6df3779f-56cf-6fb0-ac5c-0be6f8e8674e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=07de4de234ba810e289523796a39288d)

また、 `クールダウン` と呼ばれるスケールアウト/インアクティビティが完了してから別のアクティビティが開始されるまでの時間を指定する事が可能です。  
なお、 ***スケールインのクールダウンの場合、クールダウン期間中に別のアラームによってスケールアウトがトリガーされると、Service Auto Scaling によって即座にスケールアウトされます。  
スケーリングイベントが発生している最中に、別のスケーリングイベント(80%から更に90%以上になった場合)が発生し得る場合、クールダウン時間を減らす事で突如発生する `スパイク` にも対応が可能となります。***

- ***ターゲット追跡スケーリング***

特定の CloudWatch メトリクスのターゲット値に基づいてリソースをスケールします。  
ステップスケーリング同様に以下３つのメトリクスを選択し、ターゲット値を指定します。

- ***ECSServiceAverageCPUUtilization***  
	サービスの平均 CPU 使用率
- ***ECSServiceAveregemorutilization***  
	サービスのメモリ平均使用率
- ***ALBRequestCountPerTarget***  
	Application Load Balancer ターゲットグループ内のターゲットごとに完了したリクエスト数

`ターゲット追跡スケーリング` では、選択したメトリクスのターゲット値を標準としたアラームの自動生成が行われます。

例えば `ECSServiceAverageCPUUtilization` の `ターゲット値を60%` とした場合、スケールアウト/インの閾値(%)が、自動で作成され、60%を保とうとする仕組みです。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/dd00a92d-6e89-b4dc-1566-5e8f8485a81a.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Fdd00a92d-6e89-b4dc-1566-5e8f8485a81a.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=7aaa98f0a469160cfe1747d488eaf185)

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/6a78d064-7240-ef50-3cd1-d628f69440dd.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F6a78d064-7240-ef50-3cd1-d628f69440dd.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=70bd54a49c76bf7917e5af32caf00fc5)

***サービス可用性の影響を軽減させるため、スケールインポリシーは長期間にわたって実行されるようです。(X分間のXデータポイントのX値がスケールアウトポリシーの評価対象より長く設定される)***

## Capacity Provider

ECSクラスターでは、 `Capacity Provider` と、 `Capacity Provider strategy` と呼ばれる実行されたタスクをスケーリングする際に、リソースを指定する機能が備わっています。  
AutoScaling の戦略に合わせて、Capacity Providerを使用して、コストを削減する事も可能です。

### Capacity Provider

ECSクラスターに関連付けられ、後述の `Capacity Provider strategy` に使用されます。  
タスクが実行されるインフラストラクチャがFargateであれば `Fargate` あるいは `Fargate spot` を選択します。EC2であれば既存の `Auto Scaling group` をラップする形で使用します。

| 起動タイプ | キャパシティプロバイダー |
| --- | --- |
| EC2 | Auto Scaling Group |
| Fargate | FARGATE, FARGATE SPOT |

`ECS on EC2` におけるCapacity Providerは、指定した `AutoScaling Group` をラップします。起動タイプがEC2に特化した、Capacity Providerのパラメーターは主に２つです。

- ***Managed Scaling***  
	有効な場合、Auto Scalingのスケーリングプランを使用して Auto Scaling Groupのスケールアウト/インのアクションを管理します。無効になっている場合、Auto Scaling Groupを自分で管理します。
- ***Target Capacity***  
	Managed Scalingが有効になっている場合に指定可能で、1～100%をターゲット値として指定する事で、先述したターゲット追跡スケーリングポリシーを使用する事が出来ます。

一方、Fargateの場合、 `FARGATE SPOT` が存在します。  
FARGATE SPOTは通常のFARGATEよりもコストが安く、通常の FARGATEに比較して `最大70%` 割引されます。但し、Spot の在庫が少なくなってくるとタスクの停止されることがありますので、ある程度仕組み化が必要なようです。

### Capacity Provider strategy

`Capacity Provider strategy` を指定する事でCapacity Provider が有効となります。Capacity Provider strategyは、２つのパラメーターを指定する事で起動する割合を指定する事が出来ます。これらに FARGATEと FARGATE SPOT を指定する事で有効となります。

| パラメーター | 説明 |
| --- | --- |
| Base | 最小起動数 |
| Weight | 比率 |

例として、FARGATEおよびFARGATE SPOT を使用するCapacity Providerを指定します。  
最低タスク数を `2` とした場合、以下のような割合で定義すると、Scaleoutしたタスクは、FARGATE SPOTにて起動します。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/90fb8c27-a9df-72f4-a56d-92135abd8183.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F90fb8c27-a9df-72f4-a56d-92135abd8183.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=299a53409bd78ac3bc74366a7396d73d)

| CapacityProvider/Service Auto Scaling | FARGATE | FARGATE SPOT |
| --- | --- | --- |
| ***Base*** | 2 | \- |
| ***Weight*** | 1 | 1 |
| ***Service Auto Scaling*** | 2 | (Scaleout policy on) |

## CI/CD

コンテナを使用するシステムの開発、運用工程では、再現性のあるコンテナの継続的にデプロイをする、あるいはテスト・ビルド・デプロイの自動化をするためなどの目的により、CI/CDの考え方を用いた事例が多いかと思います。

AWSでは、Codeサービスと呼ばれる `CI/CDパイプライン` を構築する事で、CI/CDをAWS内で完結させることが出来ます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/31766b61-614e-dbad-358d-6f8d8deee4a0.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F31766b61-614e-dbad-358d-6f8d8deee4a0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=84156c3200d9f9409f9f90457a6950ae)

今回はECSを使用したシステムのため、ECSへ特記した事項として2点あります。

***1\. CodeBuildでビルドしたイメージはbuildspec.ymlで定義した情報からECRへイメージをpushが出来ます。（ECRのサービスロールからCodeBuildからのアクセスを許可する必要があります)***

***2\. ECR に格納されているイメージへの変更を検出し、CodeDeployを使用して、ECS クラスターとロードバランサーにルーティングしてデプロイする事が出来ます。***

これらを踏まえて、運用観点からどのような設計を行うかを考えていきます。

### パイプライン設計とイメージのメンテナンス

CI/CD パイプラインからイメージの更新がある場合、ECRへイメージを `自動プッシュ` する流れとなります。プロダクションを意識したCI/CD パイプラインでは、本番・検証・開発環境が共存するサービス、分離すべきサービスが混雑する可能性が高いため、環境単位、タスク単位などの細かなパイプライン設計が事前に必要となってくると思います。

その一つとして、 `ECRリポジトリ` をどのように分けるなども考慮が必要です。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/36f69d47-2f29-f327-e805-fa9a57e1e5c0.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F36f69d47-2f29-f327-e805-fa9a57e1e5c0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=9a782dc8f515f70ae3b1cfa6bdeac431)

ECRのリポジトリにプッシュされたイメージの実態は `S3` へ保存されますが、このリポジトリに対して、 `ライフサイクルポリシー` を設定する事が可能です。  
ライフサイクルは `ルール` に沿って解釈され、イメージの `タグ` に基づいたポリシーを設定する事も可能です。開発規模が大きければ大きいほど、イメージの更新頻度は上がりますので、ライフサイクルも同時に踏まえた上で全体のリポジトリの設計を行い、不要なファイルをメンテナンスする仕組みを設けた方がよさそうです。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/02de8199-236b-2650-4fde-7d8c83f36cb8.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F02de8199-236b-2650-4fde-7d8c83f36cb8.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=6ecd94619caccac81f7cd7ad2ce17c98)

sample4-template-ecr-lifecycle-rule.json

```json
{
    "rules": [
        {
            "rulePriority": 1,
            "description": "Expire images older than 10 days",
            "selection": {
                "tagStatus": "untagged",
                "countType": "sinceImagePushed",
                "countUnit": "days",
                "countNumber": 10
            },
            "action": {
                "type": "expire"
            }
        }
    ]
}
```

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/a316159c-6c56-1eee-7ccc-fced3ccd01aa.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Fa316159c-6c56-1eee-7ccc-fced3ccd01aa.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=ec113cc704b2c4861dbca258ce122204)

sample5-template-ecr-lifecycle-rule.json

```json
{ 
"rules": [
     { 
"rulePriority": 1, 
"description": "prod Expire images older than 100 days",
"selection": { 
              "tagStatus": "tagged", 
              "tagPrefixList": ["prod"],
              "countType": "sinceImagePushed",   
              "countUnit": "days", 
              "countNumber": 100
             },
"action": { "type": "expire" } 
             },
{ "rulePriority": 2, 
"description": "stageanddev Expire images older than 10 days", 
"selection": { 
                       "tagStatus": "tagged", 
                       "tagPrefixList": ["stage","dev"],
                       "countType": "sinceImagePushed",   
                        "countUnit": "days", 
                        "countNumber": 10
                     }, 
"action": { "type": "expire" } 
                    }
          ]
 }
```

### 承認プロセスの利用

`CodePipeline` には承認プロセスを追加する機能が存在します。

***CI/CD パイプライン経由でプロダクション環境を新しいリジョンで適用する際に、承認プロセスを設けてリリース可否を判断させる仕組みを設ける事で、開発者/運用者と承認者の間でリリースの影響下をお互いに共有する事ができ、予期せぬアクシデント未然に防ぐ、あるいは自動化に意思決定の判断を設ける、ガバナンスの強化といった様々な観点を保護します。***

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/25f24d4d-2d04-bd1d-0165-fe3e6e5f915f.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F25f24d4d-2d04-bd1d-0165-fe3e6e5f915f.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=aad785206b0f02798283d7efd54c18c0)

Code Pipeline のGUIとApproval アクションを追加する際の設定項目です。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/7c1d94f5-b545-73e0-f06f-d94f243bb153.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F7c1d94f5-b545-73e0-f06f-d94f243bb153.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=2cf41797e03d210709c86af07336158b)

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/5efe19af-fff9-9189-3c27-9fef74211c47.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F5efe19af-fff9-9189-3c27-9fef74211c47.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=8000285c7b9f3e835a0018db4f466477)

| 設定項目 | 説明 |
| --- | --- |
| ***アクション名*** | 承認アクションの任意の名前 |
| ***アクションプロバイダー*** | 承認プロセスでは手動承認を選択 |
| ***SNS トピックの ARN*** | 承認アクションの通知を送るために使用するトピック |
| ***レビュー用 URL*** | レビュー担当者に提供する URL |
| ***コメント*** | メールやコンソールでレビュアーに向けて表示出来るコメント内容 |
| ***変数の名前空間*** | パイプラインアクションで使用できる変数 |

AWS CI/CDの各サービスの概要はこちらに少しまとめています。

## Logging

コンテナのログ出力は、ドライバーにより異なります。

- ***Fargate 起動タイプを使用するタスクの場合、サポートされるログドライバーは `awslogs` 、 `splunk` 、 `awsfirelens` です。***
- ***EC2 起動タイプを使用するタスクの場合、サポートされるログドライバーは `awslogs` 、 `fluentd` 、 `gelf` 、 `json-file` 、 `journald` 、 `logentries` 、 `syslog` 、 `splunk` 、 `awsfirelens` です。***

今回は AWSに標準で使用される `awslogs` と `awsfirelens` について記載していきます。

### Cloudwatch Logs によるログ運用

`awslogs` は、コンテナのログを `Cloudwatch Logs` へ転送します。  
Cloudwatch Logs に転送されたログをエラーハンドリングする事も、ログストレージとして使用する事も可能です。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/dd82d6c7-bea2-b645-1dee-224523239a17.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Fdd82d6c7-bea2-b645-1dee-224523239a17.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=dbeba2bbd873f260320e22fd4b119481)

使用方法は、 `Task Definition` のコンテナ定義に定義された `logConfiguration` パラメーターから `logDriver:awslogs` を指定し、有効化します。タスク定義から作成された各コンテナのログはCloudWatch Logs へ転送されます。

sample6-task-Definition.json

```json
{
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "awslogs-wordpress",
                    "awslogs-region": "us-west-2",
                    "awslogs-stream-prefix": "awslogs-example"
                }
            },
            "logConfiguration": {
                "logDriver": "awslogs",
                "options": {
                    "awslogs-group": "awslogs-mysql",
                    "awslogs-region": "us-west-2",
                    "awslogs-stream-prefix": "awslogs-example"
                }
            }
        }
    ],
    "family": "awslogs-example"
}
```

***Cloudwatch Container Insightsを有効化した場合にも CloudWatch Logsロググループが生成され、転送を行います。サンプルを以下に表示します。***

sample7-Cloudwatch-Container-Insights.json

```json
{
    "Version": "0",
    "Type": "Task",
    "TaskId": "xxxxxxxxxxxx",
    "TaskDefinitionFamily": "xxxxxx",
    "TaskDefinitionRevision": "4",
    "ClusterName": "fargate-cluster",
    "AccountID": "xxxxxxxx",
    "Region": "ap-northeast-1",
    "AvailabilityZone": "ap-northeast-1d",
    "KnownStatus": "RUNNING",
    "LaunchType": "FARGATE",
    "PullStartedAt": 1643191009843,
    "PullStoppedAt": 1643191018055,
    "CreatedAt": 1643190996073,
    "StartedAt": 1643191019944,
    "Timestamp": 1643659440000,
    "CpuUtilized": 0.032220646540323895,
    "CpuReserved": 256,
    "MemoryUtilized": 15,
    "MemoryReserved": 512,
    "StorageReadBytes": 0,
    "StorageWriteBytes": 0,
    "NetworkRxBytes": 0,
    "NetworkRxDropped": 0,
    "NetworkRxErrors": 0,
    "NetworkRxPackets": 129716,
    "NetworkTxBytes": 4,
    "NetworkTxDropped": 0,
    "NetworkTxErrors": 0,
    "NetworkTxPackets": 30343,
    "EphemeralStorageReserved": 21.47,
    "CloudWatchMetrics": [
        {
            "Namespace": "ECS/ContainerInsights",
            "Metrics": [
                {
                    "Name": "CpuUtilized",
                    "Unit": "None"
                },
                {
                    "Name": "CpuReserved",
                    "Unit": "None"
                },
                {
                    "Name": "MemoryUtilized",
                    "Unit": "Megabytes"
                },
                {
                    "Name": "MemoryReserved",
                    "Unit": "Megabytes"
                },
                {
                    "Name": "StorageReadBytes",
                    "Unit": "Bytes/Second"
                },
                {
                    "Name": "StorageWriteBytes",
                    "Unit": "Bytes/Second"
                },
                {
                    "Name": "NetworkRxBytes",
                    "Unit": "Bytes/Second"
                },
                {
                    "Name": "NetworkTxBytes",
                    "Unit": "Bytes/Second"
                },
                {
                    "Name": "EphemeralStorageReserved",
                    "Unit": "Gigabytes"
                }
            ],
            "Dimensions": [
                [
                    "ClusterName"
                ],
                [
                    "ClusterName",
                    "TaskDefinitionFamily"
                ]
            ]
        }
    ]
}
```

***ログの転送は、ECS Task Role、ECS Task execution Role上に CloudwatchLogsのアクセス権限を付与する必要があります。***

### AWS FireLens によるログ運用

`FireLens` は、コンテナから直接 `Cloudwatch Logs` へ転送せず、FireLensコンテナをTaskへ追加する事でFireLensコンテナを経由したログ送信を行います。FireLensは、CloudWatch Logs への転送経路だけではなく、 `S3` や `Redshift` などにも転送が可能です。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/2ebd4770-823a-f3a8-7eb6-a6bca78ea44e.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2F2ebd4770-823a-f3a8-7eb6-a6bca78ea44e.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=43a6149eba1af6a341ab0c2b0922542b)

FireLensを使用する場合、ログルーティングとしての機能を果たすため、 `Fluentd` または `Fluent Bit` のコンテナイメージを使用する必要がありますが、 `ECR Public Gallery` 上にfluent bitのイメージが公開されているため、タスク定義に他のアプリケーション用のコンテナと同梱する事が可能です。  
***なお、Fluent Bit は、リソース使用率が Fluentd よりも低いとされています。***

タスク定義で FireLensの設定を指定する場合、 `logDriver:awsfirelens` を指定して、Name:cloudwatch\_logs/kinesis\_firehose/kinesis\_streamsを指定します。

sample8-task-definition.json

```json
"logConfiguration": {
                 "logDriver":"awsfirelens",
                 "options": {
                    "Name": "kinesis_firehose",
                    "region": "us-west-2",
                    "delivery_stream": "my-stream"
                }
            }
```

なお、FireLensを使用して出力先を分岐する場合、 `fluent Bit conf` を変更する必要があります。

sample9-firelens.conf

```text
[INPUT]
    Name xxxxx
    Listen xxxx
    Port xxxxx

[FILTER]
    Name xxxxxxxx
    Match xxxx
    Record xxxxx

[OUTPUT]
    Name firehose
    Match xxx-firelens*
    delivery_stream xxx
    region ap-northeast-1
```

***ECS Task Role、ECS Task execution Role上に各サービスのアクセス権限を付与する必要があります。***

## Trace

コンテナサービスでは、サービス間の通信や、アプリケーションへのリクエストを処理する情報が煩雑します。  
`X-Ray` は、アプリケーションへのリクエストとレスポンスの情報、呼び出しの詳細な情報などを収集し、問題の特定や可視化を行うサービスとなります。  
ECS上でX-Rayを使用する場合、 `FireLens` 同様に `サイドカー構成` として、X-rayコンテナをタスク定義にコンポーネントします。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/f404cc94-2218-1469-ac98-72d42ab03ea0.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Ff404cc94-2218-1469-ac98-72d42ab03ea0.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=fd6ec8c446dd4d51cd2e040035927e89)

X-ray デーモンからX-rayに対してデータを送信するためには以下の点をクリアにする事で実装が可能です。

- デーモンからX-Rayにアクセス許可を与えるには、SDK で `AWS認証情報(credentials)` を許可します。
- `ECS Task Role` から、X-rayの書き込み権限を許可する必要があります。
- X-ray への到達経路にはVPC エンドポイントが必要となります。

なお、X-ray には `インサイト` と呼ばれる機能があります。  
インサイトでは、アプリケーションパフォーマンスの異常を自動的に検出します。  
これにより、Amazon EventBridge イベントを経由して特定の閾値に対して発生した問題をメッセージレベルで確認する事が出来ます。

[![image.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/1597898/d8b2af3c-4053-7baf-8e2b-6457ffad41f3.png)](https://qiita-user-contents.imgix.net/https%3A%2F%2Fqiita-image-store.s3.ap-northeast-1.amazonaws.com%2F0%2F1597898%2Fd8b2af3c-4053-7baf-8e2b-6457ffad41f3.png?ixlib=rb-4.0.0&auto=format&gif-q=60&q=75&s=b636883014c0f0fb802041a582225d4c)

今回は、Service Discovery や Appmesh について触れられていませんでしたが、別の機会の記事を作成しようと思います。

## 参考文献

本記事は、以下の文献を参考とさせていただきました。

- AWSコンテナ設計・構築［本格］入門
- Amazon Elastic Container Service ドキュメント

[0](https://qiita.com/Shmwa2/items/#comments)

コメント一覧へ移動

X（Twitter）でシェアする

Facebookでシェアする

はてなブックマークに追加する

新規登録して、もっと便利にQiitaを使ってみよう

1. あなたにマッチした記事をお届けします
2. 便利な情報をあとで効率的に読み返せます
3. ダークテーマを利用できます
[ログインすると使える機能について](https://help.qiita.com/ja/articles/qiita-login-user)

[新規登録](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2FShmwa2%2Fitems%2Fbe5caef0407cb4746517&realm=qiita) [ログイン](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2FShmwa2%2Fitems%2Fbe5caef0407cb4746517&realm=qiita)

## Qiita Conference 2025 Autumn 11月5日(水)~7日(金)開催！

![](https://cdn.qiita.com/assets/public/official_campaigns/qiita_conference_2025_autumn/image-conference_2025_autumn_ogp-7cf3021de31b9ab76a7b3bbaf2909bb5.png)

Qiita Conferenceは、AI時代のエンジニアに贈るQiita最大規模のテックカンファレンスです！

基調講演ゲスト(敬称略)

piacere、牛尾 剛、Esteban Suarez、和田 卓人、seya、ミノ駆動、市谷 聡啓、からあげ、岩瀬 義昌、まつもとゆきひろ、みのるん and more…

[イベント詳細を見る](https://qiita.com/official-campaigns/conference/2025-autumn)

[123](https://qiita.com/Shmwa2/items/be5caef0407cb4746517/likers)

いいねしたユーザー一覧へ移動

101