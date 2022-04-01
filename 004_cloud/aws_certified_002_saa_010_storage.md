# Storage

# EFS

## 概要

- 複数のEC2インスタンスからアクセス可能な共有ストレージ。
- NASに似たファイルストレージ。
- ファイルシステムとして利用可能

### 特徴

- フルマネージド型サービス(運用、冗長性をAWSで担保)
- NFSv4プロトコルでアクセス
- スケーラブル
  - ペタバイトまでスケーラブルにデータ蓄積
  - スループットやIOPS性能を自動的にスケーリングし、低レイテンシを維持
- 柔軟性
  - ファイルの増減に合わせて自動で拡張・縮小
  - 事前に容量設定は不要。
  - 使用した分だけ従量課金。

### 基本性能

- 基本性能は100MB(バースト込み)
- ファイル名は255バイト
- 1ファイル当たり最大容量は48TB
- インスタンス当たり128ユーザーまでの同時オープンに対応
- 何千もの同時アクセスが可能
- 制限
  - アカウント当たりのファイルシステム数: 1000
  - AZごとのファイルシステムあたりのマウントターゲット(アクセスポイント): 1
  - ファイルシステムあたりのタグ: 50
  - マウントターゲットあたりのSG: 5
  - ファイルシステムあたりのVPC数: 1
  - 各VPCのマウントターゲット数: 400

### EFSのデータ保存

- 複数AZに分散して保存される。

### EFSの設定

- 以下の順序
  - ファイルシステム作成
  - 接続先となるマウントターゲットの作成
  - SGの作成
  - パフォーマンスモードの選択

### マウントターゲット

- 各AZにおけるEFSの接続点
- EC2インスタンスは、同じAZ内のマウントターゲットから接続
- マウントターゲットは、固定のDNS名とIPアドレスを有する。
- ファイルシステムをDNS名を使用してマウントすることで、IPアドレスを自動割り当て。

### パフォーマンスモード

- 汎用モードと最大IOモードから選択
- 汎用モード
  - 一般的なモードで、デフォルト・推奨もこちら。
  - 1秒あたりのファイルシステム操作は7000に制限
- 最大IOモード
  - 同時アクセス数のおおい場合に使用。
  - 合計スループットを優先してスケール。
  - そのためレイテンシーが少し長くなる。

### バースト機能

- 負荷に応じて自動的にスケールする。
  - サーバー容量の増大に応じたスループット性能の向上
  - 一時的なピーク負荷

- バースト機能について
  - 処理性能を一時的に向上させる
  - ファイルシステムが大きくなるに従いAmazon EFSによりスループットが拡張
  - クレジットシステムによりバースト可否が決まる

- 容量に応じたバースト性能
  - 1TB以上の容量に応じて性能が向上し、最大で1GBまでバースト可能
  - クレジットの蓄積量やスループットも容量に応じて向上

### EFSクライアント

- EFSをEC2から操作する際のクライアント
- 2種類が使用可能
  - AWSが提供するAmazon-efs-utilsに含まれるEFSマウントヘルパー
  - 通常のLinux NFSv4クライアント

### プロビジョンドスループット

- 一貫したスループットを事前に設定する。
- API, CLI, マネコンで制御可能
- スループット性能を減少できるのは、1日に1回のみ。

### EFSのユースケース

- 複数のEC2インスタンスをデータ共有する際に選択。
- 具体例
  - アプリケーションの共有ディレクトリ
  - ビッグデータなどの分散並列処理環境における共有データアクセス用のストレージ
  - コンテンツの共有レポジトリ

### その他のファイルストレージ

- Amazon FSx For Windows File Server
  - Windows 上に構築され、 Windows AD、OSやソフトウェアとの連携が豊富に可能
- Amazon FSx For Lustre
  - Lusterと互換性のある分散型の高速ストレージ
  - 機械学習などの高速コンピューティングのデータレイヤーに利用する一時保存用の処理用ストレージ

### Amazon FSx For Windows File Server

- ユースケース
  - Windows File Server のクラウド移行
  - Active Directory (AD) 統合などの幅広い管理機能
  - SMB プロトコルにより幅広く接続可
    - Amazon EC2 、VMware Cloud on AWS 、 Amazon WorkSpaces 、 Amazon AppStream 2.0 インスタンス など
  - 最大数千台のコンピューティングインスタンスからアクセス可能

- アーキテクチャ
  - ENI経由でアクセス
  - VPC、セキュリティグループでの制御
  - 単一 AZ の単一サブネットを指定して構成する。
  - 複数インスタンスでの共有や他 AZ 内のインスタンスからのアクセスも可能
  - マルチ AZ 構成を実施することもできる。

### Amazon FSx For Lustre

- 高速コンピューティング処理を実現する分散・並列処理専用の超高性能ストレージを提供
- ユースケース
  - 多くのスーパーコンピューターに利用される分散ファイルシステム
  - フルマネージド型で安全に Lustre 利用
  - 最適容量 3600GB
  - 最大数百 GB/ 秒のスループット
  - 数百万 IOPS までスケール可能

- アーキテクチャ
  - ENIまたはエンドポイント経由でアクセス
  - セキュリティグループで制御
  - 単一 AZ の単一サブネットを指定して構成する。
  - Amazon S3 とシームレスな統合
    - データレイク型のビッグデータ処理や解析ソリューションに組み込む
    - Lustre自体はあくまで一時保存用途

# ビッグデータへの対応

## AWSへのデータ量への対応

- 効率的なデータ蓄積
  - S3, Clacier, Glue, etc
- ストリームデータ処理
  - Kinesis
- 大量データの解析手法
  - Athena, EMR, Quicksight, Redshift

## ビッグデータに必要な技術

- Volume(大量データ)
- Variety(多様なデータ)
- Velocity(高速な処理)

## データレイクの利用

- 生データを使うことが多いため、データレイクが有用。
  - 構造化データでなくとも保存することができる。
  - 事前に構造(スキーマ)を決めたり、変換方法を決める必要がない
    - DWHは決めてある程度目的も明確

## ビッグデータ処理

- Hadoop
  - 大量データのバッチ処理向け
- Spark
  - ストリーミング処理向け

## AWSのサービス

- Stream保存: Kinesis
- データレイク: S3
- ETL: Glue
- Hadoop,Spark: EMR


# Kinesis

## Kinesisの概要

- ストリームデータを収集・処理するためのフルマネージドサービス
- 3つのサービスで構成される
- Amazon Kinesis Data Streams
  - ストリームデータを処理するアプリケーション構築
- Amazon Kinesis Data Firehose
  - ストリームデータをS3やRedshiftに配信
- Amazon Kinesis Data Analytics
  - ストリームデータを標準的なSQLクエリでリアルタイムに可視化・分析

### Amazon Kinesis Data Streams

- 基本的な仕組み
  - Shardに分けて分散処理し、高速処理が可能
  - Shardにインスタンスを割り当てる

- ProducerとConsumerに様々なサービスが利用可能
  - Producer
    - Kinesis Appender
    - Kinesis Producer
    - Kinesis Agent
    - AWS SDK
    - CloudWatch
    - Fluentd
    - AWS IoT
  - Consumer
    - Kinesis Firehose
    - Kinesis Client
    - Kinesis Analytics
    - Lambda
    - EMR
    - Apache Storm

### Kinesisのデータ処理順序

- 通常Kinesisはデータの処理順序を維持しない。
- パーティションキーを利用することで割り当てられたShardにおいては順序を維持したまま送信することができる。
- 送信順序が重要な場合はパーティションキーを使用する

### 様々な機能

- Kinesis Agent
  - OSSのスタンドアロンJavaアプリケーション
  - 収集するために組み込みエージェント
- Kinesis Producer Library(KPL)
  - 送信する際の補助ライブラリ。AWS SDKなどで使用できる。
- Fluentd plugin for Amazon kinesis
  - Fluentdのログ出力をKinesisのStreamsやFirehoseに送信するためのプラグイン
- Kinesis Data Generator(KDG)
  - StreamsやFirehoseにテストデータを簡単に送信できる機能（検証用）
- Kinesis Client Library(KCL)
  - KCLを使ってアプリケーションを構築
  - これはコンシューマー側のアプリケーション

### Kinesis Data Firehose

- 各種DBに配信・蓄積するためのストリーム処理を実施する。
- Lambda と連携すると ETL としても機能する(ETL用のlambdaを作成して設定する)
- Glueなどで変換処理をすることも可能
- 保存先は、S3、Redshift、ElasticSearchなど

### Amazon Kinesis Data Analytics

- FirehoseやStreamsのデータを標準的なSQLクエリでリアルタイムに可視化・分析
- 解析結果をFIrehoseやStreamsに流すことも可能。

## Kinesis Data Streamsの設定

- ストリームの容量
  - オンデマンドとプロビジョンドを選択可能
  - プロビジョンドの場合、Shard数を設定する。
- データ保持期間はあくまで一時的なものであるため、デフォルトは1日となる。
  - 後から変更可能

## 拡張ファンアウト

- シャード毎のスループットを向上させる仕組み
- コンシューマを登録する必要がある。
- 有償オプション。

# AWS Active Directory

## ユーザー管理サービス

- AWS Directory Service
  - AWS上で Active Directory を利用して、 Active Directory によるユーザー管理に用いられる。
  - またはオンプレミス環境の AD との連携やシングルサインオンを実現するためのサービス。
- Amazon Cognito
  - WEBアプリケーションやモバイルアプリケーションにユーザーサインアップ サインインおよびアクセスコントロール機能を提供。
  - Facebook 、 Google 、 Amazon などのソーシャル ID プロバイダー、および SAML 2.0 によるエンタープライズ ID プロバイダーを使用したサインインが可能になる
- AWS Single Sign On(SSO)
  - 複数のAWS アカウントとビジネスアプリケーションへのアクセスの一元的な管理を容易にし、シングルサインオンアクセスをユーザーに提供するサービス。
  - AWS Organizationsにある全アカウントに対するアクセスとユーザーアクセス許可を一元管理する
- AWS Security Token Service(STS)
  - IAMユーザーまたは AD によって認証されたフェデレーションユーザーに対して、一時的な制限付き特権の認証情報をリクエストを可能にするサービス

## AWS Directory Service

### Simple AD

- IAMユーザでなくとも簡易にAmazon Workspaceに認証・アクセスできる。
- IAMユーザは開発用
- Workspaceの仮想デスクトップを利用したい人のみ

### AD Connector

- オンプレなど既存のADを使って、AWS環境へのアクセスを可能にする。

### AWS Managed Microsoft AD

- フル機能のMicrosoft Active DirectoryをAWSで使用できる。
- SAMLなどのフェデレーションしたり、オンプレとのADの連携も可能。

### AWS SSO

- SAML2.0を使ったIDフェデレーションを使ってSSOを実現している。
- IDPという機能を使って、Simple ADやMicrosoft ADとSSOを実現している。

### Security Token Service (STS)

- 以下で一時的なセキュリティ認証情報を提供するサービス
  - IDフェデレーション
    - カスタムフェデレーションブローか
    - SAML2.0をりようしたフェデレーション
    - ウェブIDフェデレーション
  - クロスアカウントアクセス
  - IAMロール

### Cognito

- アプリケーションにユーザー認証情報を付与したい場合はCognitoを利用する。

# Amazon FSx

- 業界標準のファイルストレージを提供するフルマネージド型のストレージサービス
- ファイルストレージは、高性能ワークロード用

## FSxのタイプ

- Amazon FSx For Windows File Server
  - Windows File Server と互換性のあるストレージ
  - Windows 上に構築され、 Windows AD 、 OS やソフトウェアとの連携が豊富に可能
- Amazon FSx For Lustre
  - 分散型ファイルストレージであるオープンソース Lusterと互換性のある分散型の高速ストレージ
  - 機械学習などの高速コンピューティングのデータレイヤーに利用する一時保存用の処理用ストレージ
- Amazon FSx for NetApp ONTAP
  - NetApp の一般的な ONTAP ファイルシステム上に構築されるストレージ
  - 機能が豊富で高性能で信頼性の高いストレージを提供
  - 様々なプロトコルに対応(NFS, SMB, iSCSI)
  - マルチAZ配置が可能
- Amazon FSx for OpenZFS
  - OpenZFS ファイルシステムと AWS Graviton プロセッサ上に構築されたる高性能なファイルストレージ
  - アプリがすでに ZFS スナップショット、ファイルシステム、クォータ、予約などに依存している場合 に利用