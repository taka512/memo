---
title: "Amazon ECS Managed Instanceは AWS Fargate と何が違うのか？"
source: "https://iselegant.hatenablog.com/entry/2025/10/03/120315"
author:
  - "[[iselegant (id:iselegant)]]"
published: 2025-10-03
created: 2025-10-15
description: "はじめに iselegantです。 今日は2025年9月30日(US時間)に発表されたAmazon ECS Managed Instance(以降、ECSマネージドインスタンス)について、その全容とFargateとの違いの観点から特徴を解説していきたいと思います。 ECSにおけるコンピューティングの選択肢として、これまではEC2もしくはFargateのいずれかから選択可能でした。 ECSマネージドインスタンスの登場により、選択肢が3つとなったわけですが、EC2とFargate両者のメリットを享受できるバランスの良い選択肢という位置づけです。 EC2インスタンス自体はAWSの持ち物 ECSマネー…"
tags:
  - "clippings"
image: "https://cdn.image.st-hatena.com/image/scale/9e460a721e398fef6218225c18509272a047b2d7/backend=imagemagick;version=1;width=1300/https%3A%2F%2Fcdn-ak.f.st-hatena.com%2Fimages%2Ffotolife%2Fi%2Fiselegant%2F20251003%2F20251003102633.png"
---
## はじめに

iselegantです。

今日は2025年9月30日(US時間)に発表された **[Amazon](https://d.hatena.ne.jp/keyword/Amazon) ECS Managed Instance** (以降、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9))について、その全容とFargateとの違いの観点から特徴を解説していきたいと思います。 ECSにおけるコンピューティングの選択肢として、これまではEC2もしくはFargateのいずれかから選択可能でした。 ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) の登場により、 **選択肢が3つとなった** わけですが、 **EC2とFargate両者のメリットを享受できるバランスの良い選択肢という位置づけ** です。

## EC2インスタンス自体はAWSの持ち物

ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) を選択すると、ECSタスクを起動させるために、まずEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) が起動します。 これ自体は、今までのECS on EC2と変わりません。 ただ、この **EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) は [AWS](https://d.hatena.ne.jp/keyword/AWS) 側の責務で運用される** 、という点が異なります。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102633.png)

ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) の基本的な考え方は、Well-Architectedの一部観点(運用・セキュリティ・信頼性・パフォーマンス・コスト)と照らし合わせると比較的わかりやすいかと思うので、これらの観点に沿いながら、まずはざっと概要を理解していきましょう。

## 運用・セキュリティはAWS側の責務

前述の通り、EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 自体が「 [AWS](https://d.hatena.ne.jp/keyword/AWS) の持ち物」になります。 この [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) は [AWS](https://d.hatena.ne.jp/keyword/AWS) 側がセキュリティ対策を施したものが利用されます。 そのため、 **ユーザー側はそのOSレイヤーに介入することができません** 。 具体的には、以下のような運用においては、ユーザー側は関与することができません。

- [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) への [SSH](https://d.hatena.ne.jp/keyword/SSH) アクセス
- ルート [ファイルシステム](https://d.hatena.ne.jp/keyword/%A5%D5%A5%A1%A5%A4%A5%EB%A5%B7%A5%B9%A5%C6%A5%E0) の変更
- カスタムAMIの利用

また、起動された [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) のOS・セキュリティの状態を一定に保つため、 [Bottlerocket](https://aws.amazon.com/jp/bottlerocket/) をベースとし、定期的なパッチ適用でOSが自動保守されます。 この制約を受け、 **[14日毎にVMインスタンスが再起動](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/managed-instances-patching.html#managed-instances-lifecycle)** されます。そのため、 **その上で稼働するECSタスクも定期的に入れ替えが必要** となります。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102730.png)

## 可用性・パフォーマンス・コスト最適化の考慮は一部ユーザー側の範囲

EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 自体の運用・セキュリティが [AWS](https://d.hatena.ne.jp/keyword/AWS) 側の責任である一方、可用性・パフォーマンス・コスト管理に関して、一部はユーザー側の責務です。 順番に特徴を見ていきましょう。

### 可用性

可用性の一つの観点として、「EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) をどのAZで起動させるか？」といった点はユーザーが責務を持ちます。 これはECS on EC2やFargateと変わらず、ECSタスクが起動する先として、事前にサブネットを指定する必要があります。 ただ、マルチAZでサブネットを指定すれば、スケーリング時に [AWS](https://d.hatena.ne.jp/keyword/AWS) がベストエフォートでAZに分散配置してくれます。

ここで一つポイントとなるのが、 **ECSタスクのスケーリング時における、EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 自体のAZ分散の挙動** です。 筆者の検証結果を元に、スケーリングされるときの挙動をStep別で見ていきましょう。

まず、次のように、ECSサービスからECSタスクを起動することを考えます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102033.png)

ECSタスク数を1として起動すると、まずはECSタスクの起動に必要なEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) が起動します。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102046.png)

そして、その上でECSタスクが起動します。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102058.png)

ここまでの流れは特に違和感ないでしょう。 次がポイントとなりますが、ECSタスク数を2にすると、 **リソースが許す限り、同一のEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 上に起動しようとします** 。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102111.png)

つまり、 **ECSがマルチAZを重視して新規のEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) が起動するのではなく、集約（=コスト最適化）の観点が優先されて、起動していく動き** となります。 そして、更にECSタスク数を3とすると、

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102201.png)

まだEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) のリソースに余裕があるので、また同じEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 上にECSタスクが起動しました。 では更にECSタスク数を4にしましょう。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102212.png)

今度はEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) のリソースでは乗り切らないもしくは逼迫すると判断され、ここで初めて別のAZ側にEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) が起動し、そちら側でECSタスクが起動しました。 ここで、集約数が高いEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 上のECSタスクを１つ停止してみましょう。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102225.png)

ECSタスク数は4に維持しようとするため、ECSタスクは再度起動しますが、全体のAZ分散の状況を考慮して、比較的余裕のあるEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 側にてECSタスクが起動されました。

このように、詳細なスケーリングの動作内容は現時点で公開されていないものの、ECSタスクの起動に関して、最初は集約を意識しながらも、マルチAZが考慮されていく動きとなります。

### パフォーマンス

パフォーマンス観点では、稼働するEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) のスケーリング運用は [AWS](https://d.hatena.ne.jp/keyword/AWS) 側にて行われますが、 **どのような [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプを用いるかはユーザー側でコン [トロール](https://d.hatena.ne.jp/keyword/%A5%C8%A5%ED%A1%BC%A5%EB) 可能** です。 ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) では、この [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプやスケールの動きを [キャパシティプロバイダー](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ManagedInstances.html#managed-instances-capacity-providers) にて制御するのですが、大きく2つの選択肢があります。

まず１つ目が **デフォルトキャパシティプロバイダー** です。 これは、 [AWS](https://d.hatena.ne.jp/keyword/AWS) 自身がECSタスクで要求されるコンピューティングリソースをよしなに考慮しながら、汎用 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプを中心にスケールをコン [トロール](https://d.hatena.ne.jp/keyword/%A5%C8%A5%ED%A1%BC%A5%EB) するパターンです。 いわゆる万能型の位置づけであり、一般的なWebアプリケーションのような [ユースケース](https://d.hatena.ne.jp/keyword/%A5%E6%A1%BC%A5%B9%A5%B1%A1%BC%A5%B9) はこちらで概ねカバーできるでしょう。

そして2つ目が **カスタムキャパシティプロバイダー** です。 こちらは、ユーザー自身が、CPUやメモリの最小/最大、 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプ、 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) の特徴を指定し、その条件下で [AWS](https://d.hatena.ne.jp/keyword/AWS) がスケールをコン [トロール](https://d.hatena.ne.jp/keyword/%A5%C8%A5%ED%A1%BC%A5%EB) するパターンです。 デフォルトキャパシティプロバイダーと比較して、コンピューティングの運用に対する抽象度は下がります。 一方、 [GPU](https://d.hatena.ne.jp/keyword/GPU) を利用したいといったような、特定のニーズに応えることができます。

図で表すと、次のように整理できます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102308.png)

ちなみに、 **ECS [クラスタ](https://d.hatena.ne.jp/keyword/%A5%AF%A5%E9%A5%B9%A5%BF) ーには複数のプロバイダーを定義することが可能** です。 つまり、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) で利用されるこれら２つのキャパシティプロバイダーに加え、同一のECS [クラスタ](https://d.hatena.ne.jp/keyword/%A5%AF%A5%E9%A5%B9%A5%BF) ーにFargate用キャパシティプロバイダーも混ぜ込むことができます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102320.png)

ECSサービスやECSタスク毎のニーズに合わせて、どのキャパシティプロバイダーに基づいてリソースを割り当てるか定められるため、より選択肢が広がったと言えますね。

### コスト最適化

ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) では、利用しているEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプに応じて料金が発生します。 例えば、先程の可用性の例では、EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプが `c6g` でした。 仮にEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 上で稼働するECSタスク数が限定的である場合、表面的なコストはFargateのほうが安くなります。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102334.png)

一方、ECS [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 上のECSタスク集約数が多くなると、規模の経済が働き、コスト差は小さくなります。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102345.png)

EC2とFargateを比較した際の費用対効果の考え方として、少し前の記事になりますが、Newspicks CTO 安藤さんがブログで公開してくれているので、気になるかたはこちらも参照にされると良いと思います。

[tech.uzabase.com](https://tech.uzabase.com/entry/2022/12/01/175423)

## Fargateとの比較による違い

### 基本スタンス：コンピューティング要件の柔軟さ

「 [AWS](https://d.hatena.ne.jp/keyword/AWS) 側の責務で運用されるのであれば、”マネージドなコンピューティング”という意味でFargateと同じなのでは？使い分けはどうするの？」と考える方もいるかも知れません。

EC2マネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) とFargateを比較検討する際の基本スタンスとしては、 **EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 管理自体の責務は [AWS](https://d.hatena.ne.jp/keyword/AWS) 側に委ねたいが、コンピューティング要件に柔軟さを求めるか** 、という点がポイントになります。 前述の通り、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) は [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプ自体を自分たちでコン [トロール](https://d.hatena.ne.jp/keyword/%A5%C8%A5%ED%A1%BC%A5%EB) できます。 そのため、 [GPU](https://d.hatena.ne.jp/keyword/GPU) の利用やネットワークパフォーマンスといった、特定のリソース要件に対して、マネージドな選択肢が増えたと考えると良いでしょう。

### 一部構成上の制約

現状では、 **ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 上のECSサービスにおいては [Service Connectがサポートされていません](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ManagedInstances.html#managed-instances-limitations)** 。 そのため、ECSサービス間の接続関しては、ALB / NLB / GWLBによる接続、もしくはCloud Mapを利用したサービスディスカバリによる接続のいずれかになります。

### セキュリティレベル

Fargateは [Firecracker](https://firecracker-microvm.github.io/) と呼ばれるマイクロ [VM](https://d.hatena.ne.jp/keyword/VM) をベースに、 **ECSタスク毎に** コンピューティングリソースが用意されます。 一方、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 側は、あくまでも **EC2の [VM](https://d.hatena.ne.jp/keyword/VM) をベースに、複数のECSタスクが集約起動される形でコンピューティングリソースが用意** されます。 **ECSタスクレベルでワークロードを厳密に分離したい要件がある場合、Fargateのほうが適切** でしょう。

### イメージキャッシュとトータルの起動時間

FargateはECSタスク毎に起動するため、コンテナデプロイ時にイメージキャッシュが効かないという制約がありました。 一方、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) では、EC2がベースとなります。 コンピューティング部分がEC2となる場合、イメージキャッシュが働きます。 そのため、 **同一の [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) で同一のコンテナが起動する際は、キャッシュが利用され、コンテナの起動がより早くなります** 。

他方、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) では、 **コンピューティングリソースが不足した場合、追加のEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 起動が必要となるため、その分の起動時間オーバーヘッドは発生** します。

### タスクの入れ替え

ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) では、 [VM](https://d.hatena.ne.jp/keyword/VM) [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) のセキュリティ状態を維持するため、 **最大14日後にECSタスクの入れ替えが必要** になります。 一方、FargateにもECSタスクの入れ替えは [不定](https://d.hatena.ne.jp/keyword/%C9%D4%C4%EA) 期に発生しますが、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) ほど頻繁に発生しません（筆者の経験上、数カ月に1度程度）。 Fargateを採用しているアプリケーションでは、ECSタスクの入れ替え自体が許容できるケースが多いですが、 **この14日毎という定期的な運用サイクルに耐えられるか** 、というの一つの判断ポイントになります。

### 料金体系

FargateはECSタスクのCPU/メモリといったコンピューティングリソースに対して料金が発生します。 一方、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) は、起動する [VM](https://d.hatena.ne.jp/keyword/VM) [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプに応じた料金が発生します。

先程言及した通り、 **EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) で稼働するECSタスクの集約数が多いほど、ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 側の料金が費用対効果が高まります** 。 仮にデフォルトプロバイダーを選択した場合、 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) タイプの選定やECSタスクの稼働対象 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) は [AWS](https://d.hatena.ne.jp/keyword/AWS) がハンドリングするため、自然とコスト観点で最適化される挙動となるでしょう。

## ECSマネージドインスタンスを利用する際のIAM権限周り

EC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) の運用を [AWS](https://d.hatena.ne.jp/keyword/AWS) 側に委ねることになるので、必要なIAM権限を [AWS](https://d.hatena.ne.jp/keyword/AWS) 、すなわちEC2に対して [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) プロファイルとして渡さなければなりません。 EC2ではインフラスト [ラク](https://d.hatena.ne.jp/keyword/%A5%E9%A5%AF) チャ IAMロールと呼ばれるIAMの種別があります。 これは、ECS自体が [クラスタ](https://d.hatena.ne.jp/keyword/%A5%AF%A5%E9%A5%B9%A5%BF) ー内部のリソース管理を行うために必要な権限となります。 このインフラスト [ラク](https://d.hatena.ne.jp/keyword/%A5%E9%A5%AF) チャロールとして **ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 用の [AWS](https://d.hatena.ne.jp/keyword/AWS) 管理ポリシーを与えつつ、 [AWS](https://d.hatena.ne.jp/keyword/AWS) が運用で必要なIAMロールがEC2が使えるようにiam:PassRoleで伝搬させて上げる仕組みが必要** です。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003102902.png)

このあたりは次のドキュメントを参照することで詳細を確認できますが、初見は少しわかりにくいので、こちらの図で仕組みを頭に入れておくと、設定にハマったときに問題特定しやすいと思います。

[Amazon ECS infrastructure IAM role - Amazon Elastic Container Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/infrastructure_IAM_role.html)

[Amazon ECS Managed Instances instance profile - Amazon Elastic Container Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/managed-instances-instance-profile.html)

## その他、運用上のTips

ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) では、EC2の管理責務自体を [AWS](https://d.hatena.ne.jp/keyword/AWS) が担うものの、ユーザーは [AWS](https://d.hatena.ne.jp/keyword/AWS) マネジメントコンソール上からはEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) の稼働状況を確認可能です。 利用可能なオプションは限定的ですが、 **EC2の [ダッシュ](https://d.hatena.ne.jp/keyword/%A5%C0%A5%C3%A5%B7%A5%E5) ボード上からEC2 [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) 自体のシステムログが確認できます** 。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/i/iselegant/20251003/20251003103413.png)

IAM関連の [トラブルシューティング](https://d.hatena.ne.jp/keyword/%A5%C8%A5%E9%A5%D6%A5%EB%A5%B7%A5%E5%A1%BC%A5%C6%A5%A3%A5%F3%A5%B0) 時には、こちらのログを見ないと特定しずらいケースもあるので、覚えておくと良いでしょう。

## まとめ

いかがでしたでしょうか？

今回はECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) に関して、W-A [アーキテクチャ](https://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3) の観点からその特徴とFargateとの差異に関して解説しました。 筆者も実際の環境で利用してみたのですが、IAM設定周りのみ構築時にクリアできれば、要件に合わせた選択肢の幅が広がったこともあり、なかなか使い勝手が良い印象を受けました。 ちなみに [AWS](https://d.hatena.ne.jp/keyword/AWS) 公式ドキュメント上では、 **[実はFargateよりもECSマネージドインスタンスの利用を推奨](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/clusters.html)** していたりします。

> [Amazon](https://d.hatena.ne.jp/keyword/Amazon) ECS offers three infrastructure types for your clusters. Choose the type that best matches your workload requirements, operational preferences, and cost optimization goals:
> 
> [Amazon](https://d.hatena.ne.jp/keyword/Amazon) ECS Managed Instances (Recommended)

[Amazon ECS clusters - Amazon Elastic Container Service](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/clusters.html)

ECSマネージド [インスタンス](https://d.hatena.ne.jp/keyword/%A5%A4%A5%F3%A5%B9%A5%BF%A5%F3%A5%B9) は、パフォーマンスやコスト、 [AWS](https://d.hatena.ne.jp/keyword/AWS) 側への管理責任の委譲などの面から、バランスの良いオプションです。 いずれにしても、キャパシティプロバイダーを組み合わせることで、ニーズに併せてECSタスク稼働させることができます。 ECSを利用されているエンジニアの方々は、今後の [アーキテクチャ](https://d.hatena.ne.jp/keyword/%A5%A2%A1%BC%A5%AD%A5%C6%A5%AF%A5%C1%A5%E3) の選択肢として、ぜひ理解しておくと良いでしょう。

本ブログが皆さまの理解の手助けになれば幸いです。 では、また！

[Amazon ECS Blue/Green Deploymentは既存… »](https://iselegant.hatenablog.com/entry/2025/08/01/081020)