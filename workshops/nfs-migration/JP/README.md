# **AWS DataSync**

### AWS DataSyncとAWS Storage Gatewayを使ったNFSサーバーマイグレーション

© 2019 Amazon Web Services, Inc. and its affiliates. All rights reserved.
This sample code is made available under the MIT-0 license. See the LICENSE file.

Errors or corrections? Contact [jeffbart@amazon.com](mailto:jeffbart@amazon.com).

---

## このワークショップのシナリオ

あなたのデータセンターには更新時期が迫ったNFSサーバーが存在します。ほとんどのデータは数年前に作成されたもので、時折、読み出しが必要な時だけアクセスされます。新規のファイル書き込みも有りますが、それほど頻繁では有りません。データセンターのフットプリントを削減し、リソースを解放するため、あなたはNFSサーバーのデータをクラウドへ移行させようとしています。しかし、アプリケーションサーバーはNFSのデータを使用しており、まだ移行する事は出来ません。それらはユーザーへの遅延の影響を最小化するために、オンプレミスに置いておく必要が有ります。

色々と調べたところ、あなたはオンプレミスのNFSサーバーからAmazon S3へのデータマイグレーションに[AWS DataSync](https://aws.amazon.com/datasync/)が使える事を知り、更に[AWS Storage Gateway](https://aws.amazon.com/storagegateway)により、S3へ移行したデータへのオンプレミスからのNFSアクセスを提供できる事を知りました。

このワークショップでは、クラウドフォーメーションテンプレートを使用してリソースをデプロイし、デプロイしたリソースをAWSマネージメントコンソールから操作しながら、このシナリオの流れを学習します。以下の構成図のように、NFSサーバー、アプリケーションサーバー、DataSyncエージェント、File Gatewayアプライアンスが、オンプレミス環境を擬似したリージョンにデプロイされます。1つのS3バケットがNFSサーバーのデータのマイグレート先としてAWSリージョンに作成されます。

![](images/fullarch.png)

## このワークショップでカバーされる内容

- クラウドフォーメーションを使ったリソースのデプロイ
- Linux NFSサーバーの設定
- DataSyncのCloudWatchログの設定
- DtaSyncを使った数百万ファイルのマイグレーションの流れ
- 複数のDataSyncタスクによる並列処理

## 前提条件

#### AWSアカウント

このワークショップを完了させるには、ワークショップ内で登場するリージョンで、EC2インスタンス、AWS DataSync、AWS Storage Gateway、CloudFormationスタックを作成する権限を持ったAWS IAMロールを持ったAWSアカウントが必要です。

#### ソフトウェア

- **インターネットブラウザ**  – このワークショップには最新バージョンのChrome又はFirefoxを推奨します

## コスト

このワークショップを完了させるにはおよそ**3.00 USD**のコストがかかります。ワークショップ完了後は全てのデプロイしたリソースを削除し、無駄なコストを生じさせないように、モジュール最後に有るクリーンアップのガイダンスに従う事を推奨します。

## 関連するワークショップ

- [Migrate millions of files using AWS DataSync](https://github.com/aws-samples/aws-datasync-migration-workshop/blob/master/workshops/nfs-million-files)
- [Migrate to FSx Windows File Server using AWS DataSync](https://github.com/aws-samples/aws-datasync-fsx-windows-migration)
- [Get hands-on with online data migration options to simplify & accelerate your journey to AWS](https://github.com/aws-samples/aws-online-data-migration-workshop)

## ワークショップモジュール

このワークショップは以下の5つのモジュールで構成されます

- [モジュール 1](module1/)  - オンプレミスとin-cloudリージョンへのリソースのデプロイ
- [モジュール 2](module2/) - DataSyncを使用したS3への初回ファイル同期
- [モジュール 3](module3/)  - File Gatewayを使用したオンプレミスからのS3バケットへのアクセス
- [モジュール 4](module4/)  - カットオーバー前の最後の差分同期
- [モジュール 5](module5/) - File Gatewayへの完全移行とNFSサーバーのシャットダウン

始めるには [モジュール 1](module1/)へ
