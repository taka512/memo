---
title: "Visual Studio Code上で AWS Step Functions のワークフローをより簡単に構築"
source: "https://aws.amazon.com/jp/blogs/news/introducing-an-enhanced-local-ide-experience-for-aws-step-functions/"
author:
  - "[[Amazon Web Services]]"
published: 2025-03-24
created: 2025-10-15
description: "AWS Step Functions は、ステートマシンの構築をより簡単にするために、ローカル IDE 機能がより強化されました。これにより、Workflow Studio が AWS Toolkit 拡張機能を通じて Visual Studio Code (VS Code) で利用できるようになりました。開発者は AWS コンソールと同じ強力な視覚的な編集機能を使用して、ローカル IDE でステートマシンを作成および編集できます。"
tags:
  - "clippings"
image: "https://d2908q01vomqb2.cloudfront.net/b3f0c7f6bb763af1be91d9e74eabfeb199dc1f1f/2025/03/24/step-functions-8-service.jpg"
---
## Amazon Web Services ブログ

この記事は2025 年 3 月 7 日に公開された「 [Introducing an enhanced local IDE experience for AWS Step Functions](https://aws.amazon.com/jp/blogs/compute/introducing-an-enhanced-local-ide-experience-for-aws-step-functions/) 」を翻訳したものとなります。

*本記事は、シニアソリューションアーキテクトの Ben Freiberg が執筆しました。*

[AWS Step Functions](https://aws.amazon.com/step-functions/) は、ステートマシンの構築をより簡単にするために、ローカル IDE 機能がより強化されました。これにより、Workflow Studio が [AWS Toolkit](https://aws.amazon.com/visualstudiocode/) 拡張機能を通じて Visual Studio Code (VS Code) で利用できるようになりました。  
開発者は AWS コンソールと同じ強力な視覚的な編集機能を使用して、ローカル IDE でステートマシンを作成および編集できます。

Step Functions は、開発者が AWS サービスを使用して分散アプリケーションの構築、プロセスの自動化、マイクロサービスのオーケストレーション、データや機械学習 (ML) パイプラインの作成を支援するビジュアルワークフローサービスです。

お客様は、 [AWS Lambda](https://aws.amazon.com/lambda/) 、 [AWS Fargate](https://aws.amazon.com/fargate/) 、 [Amazon Bedrock](https://aws.amazon.com/bedrock/) 、 [HTTP API 統合](https://docs.aws.amazon.com/step-functions/latest/dg/call-https-apis.html) など、複数のサービスを含むワークフローを構築するために Step Functions を選択しています。開発者は、 [Workflow Studio](https://docs.aws.amazon.com/step-functions/latest/dg/workflow-studio.html) を使用して AWS コンソールから、または JSON ベースのドメイン固有言語である [Amazon States Language (ASL)](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-amazon-states-language.html) を使用してコードとして、これらのワークフローをステートマシンとして作成します。開発者は、アプリケーションやインフラストラクチャ (IaC) コードと共にワークフローの定義を管理します。今や、開発者は AWS コンソールと同じ体験で、VS Code でワークフローを構築およびテストするためのさらなる機能を利用できるようになりました。

## ローカルワークフロー開発の簡素化

統合された Workflow Studio により、開発者は自身のローカル IDE 内で Step Functions ワークフローを円滑に構築できます。開発者は AWS コンソールで使用するのと同じキャンバスを使用して、ステートをドラッグ＆ドロップしてワークフローを構築できます。ワークフローを視覚的に修正すると、ASL 定義が自動的に更新されるため、構文ではなくビジネスロジックに集中できます。 Workflow Studio の統合により、作業環境を切り替えることなく、AWS コンソールと同じ直感的で視覚的なステートマシンの設計アプローチを提供します。

## はじめに

更新された IDE の操作性を使用するには、VS Code 拡張機能として [AWS Toolkit](https://aws.amazon.com/visualstudiocode/) のバージョン 3.49.0 以上がインストールされていることを確認してください。

[![更新可能な VS Code の AWS Toolkit 拡張機能](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-1.jpeg)](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-1.jpeg)

*図 1: AWS Toolkit の更新*

AWS Toolkit 拡張機能をインストールした後、ステートマシン定義を開いて Workflow Studio でビルドを開始できます。  
ローカルワークスペースの定義ファイルを使用するか、 [AWS Explorer](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/aws-explorer.html) を使用してクラウドから既存のステートマシン定義をダウンロードできます。  
VS Code の統合機能は、JSON 形式と YAML 形式の ASL 定義をサポートしています。  
(注: Workflow Studio が自動的にファイルを開くためには、ファイルの拡張子が `.asl.json` 、`.asl.yml` 、または `.asl.yaml` である必要があります。)  
YAML ファイルを使用する場合、Workflow Studio は編集のために定義を JSON に変換し、保存前に YAML に再変換します。

[![デザインモードが開かれた Workflow Studio のサンプルステートマシン](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-2.jpeg)](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-2.jpeg)

*図 2: Workflow Studio のデザインモード*

VS Code の Workflow Studio は、デザインモードとコードモードの両方をサポートしています。デザインモードでは、ワークフローの構築と確認のためのグラフィカルインターフェイスを提供します。  
コードモードでは、統合されたコードエディタを使用して、ワークフローの [Amazon States Language](https://docs.aws.amazon.com/step-functions/latest/dg/concepts-amazon-states-language.html) (ASL) 定義を表示および編集できます。  
次の画面に示すように、Workflow Studio の右上にある *Return to Default Editor* リンクを選択することで、いつでもテキストベースの編集に戻ることができます。

[![コードモードが開かれた Workflow Studio でのサンプルステートマシン](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-3.jpeg)](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-3.jpeg)

*図 3: Workflow Studio のコードモード*

VS Code で Workflow Studio を手動で開くには、ワークフロー定義ファイルの上部にある「 Open with Workflow Studio 」アクションを使用するか、エディターペインの右上にあるアイコンを使用します。  
以下の画面では、両方のオプションが強調表示されています。  
また、ファイルエクスプローラーペインからファイルのコンテキストメニューを使用して Workflow Studio を開くこともできます。

[![デフォルトエディタで表示された asl ファイルと、Workflow Studio を開くための様々な方法](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-4.jpeg)](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-4.jpeg)

*図 4: エディターへの Workflow Studio の統合*

Workflow Studio で行った編集は、未保存の変更として元の ASL ファイルに自動的に同期されます。  
変更を保存するには、Workflow Studio またはファイルエディタから変更を保存する必要があります。  
同様に、ローカルファイルに加えた変更は、保存時に Workflow Studio に同期されます。

Workflow Studio は [Definition Substitutions](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-stepfunctions-statemachine.html#cfn-stepfunctions-statemachine-definitionsubstitutions) に対応しているため、 [AWS CloudFormation](https://aws.amazon.com/cloudformation/) や [AWS Cloud Development Kit (CDK)](https://docs.aws.amazon.com/cdk/v2/guide/home.html) などの IaC ツールと統合されたワークフローも編集できます。  
Definition Substitutions は CloudFormation の機能で、IaC テンプレートで提供する値への動的な参照をワークフロー定義に追加できます。

```yaml
AWSTemplateFormatVersion: "2010-09-09"
 Description: "State machine with Definition Substitutions"
 Resources:
  MyStateMachine:
    Type: AWS::StepFunctions::StateMachine 
    Properties:
      StateMachineName: HelloWorld-StateMachine 
      DefinitionS3Location:
        Bucket: amzn-s3-demo-bucket 
        Key: state-machine-definition.json 
      DefinitionSubstitutions:
        TableName: DemoTable
```

YAML

その後、ステートマシンの定義で定義の代替を使用できます。

```json
"Write message to DynamoDB": {
  "Type": "Task",
  "Resource": "arn:aws:states:::dynamodb:putItem",
  "Next": "Remove message from SQS queue",
  "Arguments": {
    "TableName": "${TableName}",
    "Item": {
      ... omitted for brevity ...
     }
  },
  "Output": "{% $states.input %}"
}
```

JSON

このコードは、putItem オペレーションを使用して DynamoDB にメッセージを書き込む Step Functions のタスクステートを定義しています。  
`${TableName}` という置換構文により、ステートマシンの実行時にパラメータとして渡される動的な DynamoDB テーブル名を使用できます。

## テストとデプロイ

Workflow Studio の統合により、Step Functions の [TestState API](https://aws.amazon.com/blogs/compute/accelerating-workflow-development-with-the-teststate-api-in-aws-step-functions/) を使用して単一のステートをテストできます。  
TestState API を使用すると、ステートマシンを作成したり、既存のステートマシンを更新したりすることなく、ローカルの IDE からクラウド上のステートをテストできます。  
このにより、ステートマシン全体を呼び出すことなく、個々のステートの変更の作成とデバッグができます。  
たとえば、IDE を離れることなく、入出力の処理を改善したり、Choice ステートの条件ロジックを更新したりできます。

**状態のテスト**

1. Workflow Studio で任意のステートマシン定義ファイルを開きます
2. キャンバスまたはコードタブから State （日本語: 状態）を選択します
3. 右側のインスペクターパネルが開いていない場合は開きます [![A DynamoDB PutItem opened in the Workflow Studio inspector panel with the Arguments showing a Definition Substitution..](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-5-1.png)](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-5-1.png)  
	**図 5 ：個別のステートの引数**
4. 上部の **Test state** ボタン（日本語: テスト状態）を選択します
5. IAM ロールを選択し、入力を追加します。 [TestState API を使用するために必要な権限](https://docs.aws.amazon.com/step-functions/latest/dg/test-state-isolation.html#test-state-permissions) がロールに付与されていることを確認してください。
6. ステートに Definition Substitutions が含まれている場合、特定の値に置き換えることができる追加セクションが表示されます
7. **Start Test** （日本語: テストを開始）を選択します

[![Modal popup of the TestState with a role selected and showing the entered value of a Definition Substitution.](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-6.png)](https://d2908q01vomqb2.cloudfront.net/1b6453892473a467d07372d45eb05abc2031647a/2025/03/05/sf-ide-6.png)

*図 6: 定義の置換を含むテストステートの設定*

テストが成功したら、AWS Toolkit を使用して [IDE からワークフローを公開](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/building-stepfunctions.html) できます。  
また、 [AWS Serverless Application Model](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/what-is-sam.html) 、AWS CDK、CloudFormation などのインフラストラクチャーをコードで管理するためのツールを使用してステートマシンをデプロイすることもできます。

## まとめ

Step Functions は、VS Code IDE と AWS Toolkit を使用したワークフローの開発を簡素化するため、強化されたローカル IDE 環境を導入しています。  
これにより、コーディング、テスト、デプロイ、デバッグのサイクルが効率化され、開発者は Workflow Studio をスムーズに統合できます。  
ビジュアルなワークフローデザインと充実した機能を備えた IDE のパワーを組み合わせることで、開発者はより効率的に Step Functions のワークフローを構築できるようになりました。

まず、 [AWS Toolkit for Visual Studio Code](https://aws.amazon.com/visualstudiocode/) をインストールし、Workflow Studio の統合に関する [ユーザーガイド](https://docs.aws.amazon.com/toolkit-for-vscode/latest/userguide/stepfunctions-workflowstudio.html) をご覧ください。  
AWS サーバーレスに関する実践的な例、ベストプラクティス、有用なリソースは [Serverless Land](https://serverlessland.com/) でご覧いただけます。

翻訳は技術統括本部 ソリューションアーキテクトの [菅原太樹](https://x.com/taikis_tech) が担当し、一部を加筆修正しました。