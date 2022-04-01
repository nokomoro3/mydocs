# Database

## Databaseの概要

- DBとは。DBMS
- CRUDとは。
- DBとストレージの違い
- DBの意味
  - 障害や復旧
  - 複数人のアクセス
  - 大量データの検索

- データベースの役割
  - トランザクション
    - 同時アクセス
    - 処理に失敗するとrollbackする
    - 復旧
  - データモデル

### トランザクションのACID特性

- A: Atomicity(原始性)
  - すべて実行か、一つも実行されないかどちらかの状態。
- C: Consistency(整合性)
  - データの整合性。
  - 整合性モデル
    - 結果整合性（データが古い可能性）
    - 強い整合性（データ更新中は参照できない）
- I: Isolation(独立性)
  - 実行中の過程を隠蔽し他の処理に影響を与えない。
- D: Durability(耐久性)
  - トランザクション中にクラッシュしてもデータが失われない。

### データモデル

- 様々なモデルがある
  - リレーショナルモデル
  - グラフモデル
  - キーバリューストア
  - オブジェクト
  - ドキュメント
  - ワイドカラム
  - 階層型

### 最適なデータベースの選択

- RedShift
- RDS
- DynamoDB
- Aurora
- Elastic Search

### 大きな区分け

- リレーショナルDB
  - 通常のDB
  - ER図などで表現する構造化データ
  - 会計データや顧客データなど
  - SQLで操作
- NoSQL
  - ビッグデータ向け
  - 構造化されていないKey,Valueのみ
  - 非構造化データ（文書、動画、音声）
  - 半構造化データ（XML, JSON, 文書）
  - SQLを使わないが、SQLに似た操作ができるものもある。

### NoSQLの種類

- キーバリューストア
  - 高速なパフォーマンスと分散拡張に優れている
  - 読み込みが高速
- ワイドカラムストア
  - 列指向データベース
  - 行ごとに任意のカラムを無数に格納できる。
- ドキュメントデータベース
  - Valueではなく、JSONやXMLそのものを格納
- グラフデータベース
  - グラフ理論を用いたデータベース
  - 高速な横断検索が可能

### ビッグデータの活用

- DWH
  - 構造化データ
  - 売り上げ・財務分析などのデータ分析
- アーカイブデータ
  - 構造化アーカイブデータ
  - DWHだけでは不可能な長期過去データを分析
- 半構造化
  - IoTなど膨大なビッグデータを活用した分析・AI構築
- 非構造化
  - 非構造化データを活用した分析・AI構築

### RDB

- AWSのサービスではRDSが該当

### DWH

- 概要
  - BIツールなどと連携した分析向けのデータベース
  - AWSのサービスではRedshiftが該当
  - オンプレであれば、Exadata, Teradataなど。
  - 読み込みデータ構造をあらかじめ設計して蓄積していく。
  - レスポンス重視でデータ抽出・集計が速いが、更新・トランザクションは遅い。
- アーキテクチャ
  - データをパーティショニングし、複数ディスクから読み込む
  - 列指向

### 分散型DB/データレイク

- 高速処理を可能にするDBとストレージの組み合わせ
- AWSであれば、S3がデータレイクに相当（するが、分散DBと言えるか？？）
- オンプレではApache Hive, Apache Hadoopなど

### KVS

- AWSサービスではElastiCacheやDynamoDB
- オンプレであれば、Redisなど。
- 結果整合性を採用。高速なデータ処理を重視。
- 用途
  - webサイトのバックエンドデータ（ユーザーセッションなど）
  - twitterなどのメッセージングシステム
- キーに応じてデータを分散して保存し分散処理できる。

### ワイドカラム型

- AWSサービスではDynamoDB
- オンプレではcassandra, Apache HBASEなど
- データ取得の際に結合しなく良いように、可能な限り多くのデータを同じ行に保持する。
- KVSと異なり、SQLライクなデータ操作が可能。
- 更新はできず、挿入による上書きで実現する。
- 用途
  - ソーシャルデータの位置情報データ
  - データマイニング処理
- キーとカラムが入れ子構造となっている。
- ここがワイドカラムについては分かりやすいかも。
  - https://future-architect.github.io/articles/20190718/

### ドキュメントDB

- AWSサービスでは、DocumentDB(MongoDB)が該当。
- JSONやXMLデータを格納
- 様々なデータ構造を混ぜて保存できる
  - 構造が違ってもOK
- KVSよりもクエリが豊富なため操作しやすい
- shardingによるデータベース分散化が可能

### インメモリデータグリッド

- AWSでは、Redis ElastiCacheやMemcached ElastiCacheが該当
- KVSをメモリ上で実現し高性能にしたDB
- データを多数のサーバーで分散して処理可能
- 金融の取引データなどが利用対象

### 全検索型エンジンと分散DB

- AWSではElasticSearch
- オンプレではElasticSearchやkibana
- 合分析の柔軟性や速度が高く、分析・蓄積・可視化環境を容易に構築可能
- 半構造化データを対象
- サイト内の全データ検索や検索自身の可視化など。

### グラフDB

- AWSではAmazon Neptune
- オンプレでは、Neo4j
- グラフ演算に特化したDB
- データ間のつながり方の検索・可視化に利用
- 利用対象としては、最短経路・金融取引の詐欺検出・ソーシャルネットワークの計算
- RDB以上にスケールアウトすることはできない。
- レコードが増えると検索に時間がかかる。

### 分散OLTP(RDB)

- AWSでAuroraで提供される。
- 分散型の次世代RDB。
- 高い高可用性と高性能なトランザクションと強整合性が実現
- 利用対象としては大規模な業務データ処理など

## DynamoDB

### DynamoDBの特徴

- 完全マネージド型のNoSQLデータベースサービス

- ハイスケーラブル
  - 無制限に性能を拡張でき、ストレージの増加なども不要
- 低レイテンシー
  - 負荷が高くなっても応答速度を維持
- 高可用
  - SPOFなしでデータを３つのAZに保存
- メンテナンス不要
  - マネージド型のためCloudWatchで運用
- プロビジョンドスループット設定
  - テーブルごと、Read/Writeごとに必要なスループットキャパシティの割り当てが可能。

### DynamoDBでできること

- ワイドカラム型のデータベース

- できること
  - キーに対するバリュー（値）のCRUD 操作
  - 簡易なクエリやオーダー
  - 数万人が同時にアクセスするようなセッションデータの管理など

- できないこと
  - JOIN, TRANSACTION, COMMIT, ROLLBACKは負荷
  - 詳細なクエリやオーダー(検索など)に不向き
  - 大量のデータIOにはコストがかかる。

### DynamoDBの整合性モデル

- デフォルトは結果整合性
- Write
  - 2つのAZで書き込み完了された時点で完了
- Read
  - デフォルト
    - 結果整合性
  - オプション
    - 強い整合性モデル
    - GetItem, Query, Scanでは強い整合性のある読み込みオプションが指定できる。

### DynamoDBのパーティショニング

- 一つのテーブルを複数のパーティションに分けるなど。

### DynamoDBのユースケース

- ビッグデータ向けの処理
  - 大量のデータを収集・蓄積・分析(Kinesis連携など)
  - Hadoopと連携したビッグデータ処理
- アプリケーション向け
  - 大規模サービスで高速処理が必要なアプリケーション向け
  - ユーザー数が多いアプリケーションのデータ処理
- ユーザー行動管理
  - ゲーム、広告などのユーザー行動のDB
  - transactionやrollebackがあまりいらない
- バックエンドデータ処理
  - モバイルアプリのバックエンド

### 適用分析

- JOIN, TRANSACTION, COMMIT, ROLLEBACKがあるか
- 検索, 大量の読み書きがあるかないかで判断

### テーブル設計

- テーブル
  - 項目(Item): 例としてはPersonalなど
    - 属性: 1つ以上の属性を持つ。Personalの属性として、姓、名など。
- 属性はVALUE型でもJSON型でも不揃いでよい。

### DynamoDBのインデックス

- 暗黙的なキー
  - データを一意に特定するため、ハッシュキーやレンジキーを宣言して検索に利用するキー
  - １テーブルに１つ宣言
- 明示的なキー
  - LSI(local secondary Index)
    - 追加でレンジキーを増やすイメージ
    - 1テーブルに５つ作成可能、テーブル作成時に作成
    - 検索を詳細にしたい場合などに使用。
  - GSI(global secondary Index)
    - データに対して別のハッシュキーを設定できる。
    - 1テーブルに５つ作成可能、テーブル作成後に作成

### プライマリーキー

- ハッシュキー
  - テーブル作成時に一つの属性を選択し、ハッシュキーとして宣言
  - ハッシュ関数によってパーティションを決定するためハッシュキーと呼ぶ
  - 単独での重複は許さない
- レンジキー(複合キー)
  - ハッシュキーにレンジを加えたもの
  - 2つの値の組み合わせにより項目を特定
  - 複合キーは、単独であれば重複が許される。

- ハッシュキーにもレンジキーにも、アイテムの属性を使用する。

### 明示的に利用するインデクス

- ハッシュキーやレンジキーだけで検索が満たせない場合に使用
- LSIは複合キーテーブルのに設定できる。
- GSIはハッシュキーの代わりとなるため、ハッシュキーテーブル、複合キーテーブルどちらにも設定可能
- 使用するとスループットやストレージ容量の追加が必要なため、多用すべきではない。

### テーブル操作

- 代表的なもの
  - GetItem
    - ハッシュキーを条件に一定の項目を取得
  - PutItem
    - 1件書き込み
  - Update
    - 1件更新
  - Delete
    - 1件削除
  - Query
    - ハッシュキーやレンジキーを元にした検索(最大1MB)
  - Scan
    - テーブルの全件検索(最大1MB)
  - BatchGet
    - 複数のプライマリーキーに対してマッチする項目を取得

### DynamoDB Streams

- DynamoDBテーブルに保存された項目の追加・変更・削除の発生時の履歴をキャプチャできる機能

- 過去24時間以内のデータ変更履歴を保存し、24時間経過すると消去される

- 同じハッシュキーに基づくデータの順番は維持されるが、異なるハッシュキーの場合順番が前後する可能性がある。

- ユースケース
  - クロスリージョンレプリケーション
    - streamsのキャプチャをトリガにしてレプリケーションを実行できる
  - データ更新をトリガーとしたアプリケーション機能
    - データ更新に応じた通知処理などのアプリケーション処理の実行
    - lambda関数の実行などで、別テーブルの自動更新やログ保存、プッシュ通知などを実現できる。

### DynamoDB Accelerator (DAX)

- DynamoDBにインメモリキャッシュ型の機能を付加する。
- DAXクラスタを使用して読み込む。
- マルチAZDAXクラスタを構成できる。
- マイクロ秒単位まで結果整合性のある読み込みワークロードの応答時間を短縮。
  - 金融系などが想定される用途である。
- DAXは運用上そしてアプリケーションの複雑性に影響なく容易に導入できる。

### グローバルテーブル

- DynamoDBはリージョン単位で作成するが、リージョン間で同期されるマルチマスターテーブルを作成可能
- 複数のリージョンにエンドポイントを持つことができる。
- レプリケーションにかかるデータ転送料金が必要
- 強い整合性を設定することはできなくなる。

### オンデマンドバックアップ

- パフォーマンスに影響なく数百TB のバックアップを実行可能
- 任意のタイミングで利用可能な長期間データ保存用バックアップ
- 従来はデータパイプラインを利用して取得したバックアップを容易に実施できるようになった。

### Read/Writeキャパシティオンデマンド

- キャパシティ設定不要でリクエストに応じた課金設定を選択できるようになった。
  - トラフィック量の予測が困難な場合にリクエストの実績数に応じて課金
  - オンデマンドで Read Write 処理に自動スケーリングを実施
  - プロビジョンドキャパシティ設定への変更は無制限
  - オンデマンドへの変更は 1 日 1 回まで

### DynamoDB活用

- 既存のRDB 中心のアーキテクチャを見直して DynamoDB とLambda などの組合せでサービスが実現できないか検証する

## DynamoDBのデモ

- DynamoDBはテーブルを作成する。
  - RDBはデータベースを作る。
  - S3と同様、リージョンに作成される（AZではない）。

### テーブル作成

- テーブル名の作成
- パーティションキーの設定
  - プライマリーキー。これがハッシュになる。
  - 例：社員ID
- ソートキーの設定
  - プライマリーキーのレンジキーとなる。
  - 例：部署名
- テーブルクラス
  - 標準と標準の低頻度アクセスが選択できる。
- キャパシティ計算ツール
  - 後述のキャパシティの見積もりができるツール
- キャパシティモード
  - オンデマンドとプロビジョンドを選択可能
- 読み込みキャパシティ
  - Auto Scalingを有効にすると、最大、最小のキャパシティユニットを設定可能
- 書き込みキャパシティ
  - 同上。
- セカンダリインデックス
  - LSI
    - ソートキーを追加できる。
    - ただしよりキャパシティを消費する
    - 途中で追加することができないので要注意
  - GSI
    - テーブル全体をスキャンして全体で何かを算出したい場合に使用
    - パーティションキーとソートキーが必要になる。
    - GSIは後で作成できる。
    - ただしよりキャパシティを消費する
- 保管時の暗号化設定
  - 暗号化自体は必ず設定する。


### データ追加
  - パーティションキー、ソートキーは空にすることができない。
  - セカンダリは空でも実施できる。
  - マネコンでも追加可能だが、基本はプログラムで追加していく。

### その他の機能

- モニタリング
  - CloudWatchのメトリクスを確認できる。
  - アラーム等も設定可能。

- グローバルテーブル
  - レプリカを作成することができる
  - これを作成すると、付随してDynamoDB Streamsが自動的に有効化される。

- バックアップ
  - オンデマンドバックアップを設定したり、スケジューリングすることが可能。
  - AWS Backupを使用したり、DynamoDBで実施することが可能
  - スケジューリングの場合は、AWS Backupと連携して実現する。

- ポイントタイムリカバリ(PITR)
  - データを誤って削除した場合に時点を指定して復元できる。
  - 追加の料金が必要。

- エクスポート(DynamoDB Stream)
  - DynamoDB Streamの設定。
  - Kinesisと連携してまずStreamが作成される。
  - その後、ストリームを有効化する。

- S3へのエクスポート
  - PITRを有効化すれば、S3バケットに保存することが可能。

- PartiQL
  - SQLのようなクエリを実行することができるようになっている。

- リザーブドキャパシティ
  - リザーブドで予約して購入することが可能。
  - 割引された状態で購入することができる。

### DAX

- クラスターを設定する
- T型とR型を選択できる。
- サブネットに対して作るため、サブネットを指定する必要がある。
- サブネットグループを選択(新規作成)する。
  - VPCを指定する。
    - その中のサブネットを選択する。
- アクセスコントロール
  - セキュリティグループを設定する。
- AZ割り当て
  - ノードを自動的にどのAZに割り当てるか決める。
  - 手動でも設定可能
- IAMロール設定
  - DynamoDBにアクセスする権利をクラスタに付与する。
  - Readonly, ReadWriteテーブル指定など細かい権限を指定できる。
- 暗号化
  - メモリ保持時や転送時の暗号化設定を有効化できる。
- パラメータグループ
  - 新規作成か設定する
- メンテナンスウィンドウ
  - クラスタのメンテナンスをする時間をせっていできる。

# Aurora

## Auroraの概要

- マルチAZで分散されたクラスター構成により、高速・高性能なリレーショナルデータベース
- Amazonがクラウド時代に適したリレーショナルDBを一から考えて構築された新RDB
- その特徴はNoSQL型の分散高速処理とRDBとしてのデータ操作性を両立させたこと
- MySQLと2.5～5倍の性能を商用データベースの10分の1の価格で提供する高性能・低コストDB

### 特徴

- 高い並列処理によるストレージアクセスによってクエリを高速処理することが可能
- Auroraは大量の書き込みや読み込みを同時に扱うことができる
- データベースの集約やスループット向上が見込まれる
- ただし、すべてが5倍高速というわけではなく、適用すべき領域を見つけて利用する
- MySQL5.6互換やPostgreSQL互換を選択可能

### 耐障害性／自己回復性

- 3つのAZに2つのコピーを設置可能で合計6つのコピーを保持
- 過去のデータがそのままS3に継続的バックアップ
- リストアも差分適用がなく高速
- どのタイミングでも安定したリストア時間を実現
- 99.99％の高可用性・高耐久性

### スケーラビリティ

- 10GBから最大64TBを提供するSSDデータプレーンを利用してシームレスに拡張可能
- Auto-Scalingなどのクラウド独自のスケーラブルが可能
- 最大15のリードレプリカを利用した高速読み込みが可能

### ユースケース

- 大規模なクエリ処理が発生するRDB環境などはAuroraへの移行を検討する。
  - 書込み量が多くでトランザクション量が多い
  - クエリ並行度が高い、データサイズが大きいケースで効果を発揮する
  - コネクション数やテーブル数が多いデータベース処理
- 運用の容易さを活用する
  - スケーラビリティの高さやデータ容量が無制限に拡張できる
  - レプリケーションなどの性能の高さ

### 構成

- 1つのDBインスタンスと1つのDBクラスタボリュームで構成
- 3つのAZにコピーされたクラスタを単一と認識
- Aurora Write(マスター)とAurora Reader(リードレプリカ, 最大15個)でDBクラスタを構成する。
- Write, Readそれぞれのエンドポイントを介してEC2などからアクセスする。

### フェイルオーバー

- Readerをtier番号が大きい順、サイズが大きいサイズのReaderにフェイルオーバーを実施する。

### マイグレーション

- MySQLやPostgresSQLのスナップショットからAuroraへマイグレーション可能
  - MySQLは5.6, 5.7, 8.0.23のバージョンと互換性がある

### マルチマスタ構成

- Writerを複数別AZにスケーラブルに構築可能
- どのノード、AZが落ちてもダウンタイムがゼロとなる

### AuroraグローバルDB

- 他リージョンに設置する高性能なリードレプリカ作成が可能
- ストレージレベルでレプリケーションを実施し、低レイテンシーでセカンダリリージョンとレプリケーションが可能

### 作成

- 使用可能なインスタンスクラス
 - tおよびrから選択
- 設置場所
  - VPCおよびサブネットを選択
  - セキュリティグループの設定

### カスタムエンドポイント

- エンドポイントを増やすことが可能。
- 増やすメリットは？

### DBクラスタを他のAWSアカウントと共有

- ?

### プロキシの作成

- lambdaと連携する場合はプロキシを作成する
- こちらだとコネクションが増えないというが、なぜだろう？

### AutoScalingポリシー

- RDSはストレージしかスケーリングできない。
- AuroraはEC2インスタンスのようにAutoScalingすることができる。
- 具体的には、リードレプリカを増加させたりすることができる。

### スナップショット

- S3に保存され、ユーザーが指定したS3に保存することも可能。

### クローン

- クラスタ構成自体を複製することが可能。

### 実際の接続

- mysqlコマンドがインストール済みのEC2インスタンスからエンドポイントに接続する。

### Auroraサーバレス

- ワークロードが予測困難な場合に使用
- Auroraのオンデマンド自動スケーリングを実施
- lambdaなどと連携して必要に応じて起動する。
- 普通のAurora(プロビジョンド)との違い
  - インスタンスサイズを指定できない。
  - 代わりにフリートを設定してACUをMIN,MAXを設定する。
- スケールがタイムアウトした場合や、アイドル状態の挙動も設定が必要。
- DATA APIを有効にすると接続を管理せずに、Auroraサーバレスに対してSQLクエリを実行できる。

### Auroraサーバレスのメリット

- 実働時間の変動が激しいケースでコストが安くなる。
- スケーリングや可用性面でAuroraには劣る。


# Redshift

## Redshift概要

- DWHを構成する、またはデータレイク分析をするサービス
- ペタバイト以上まで拡張可能
- 1TBあたり年間1,000USD以下で利用可能。
- 自動ワークロード管理で、自動テーブルメンテナンスなどができるフルマネージド型
- PostgresSQL互換の列指向データモデル
- クラスタ構成が可能だが、単一AZでマルチAZ構成は不可。
- 高性能なra3インスタンスを使う
- AQUAによる分散キャッシュによる高速動作が可能

### データレイクの分析

- S3にデータをため込む
- S3をデータレイクとし、Redshift Spectrumで分析

### インスタンスタイプ

- RA3またはDC2インスタンスを選択
- RA3インスタンスは1ノードあたり$3.836USD/hourだが、DC2は$0.314USD/hour
- RA3は最低2ノード必要
- 1TB未満のデータセットでは、DC2ノードの使用を推奨

### 構成

- リーダーノード
  - クエリのエンドポイント
  - SQLの生成、実行
- コンピュートノード
  - 高速SSDによるキャッシュ
  - クエリの並列実行
  - 最大３２個まで適用可能
- ストレージはRedshift管理用S3が勝手に構成

### 特徴

- 列指向
- データ圧縮
  - ブロック単位でデータを格納し、ディスクIOを効率化
- ソート
  - ブロックに対してメタデータを付与してそれを用いて検索する
  - リーダーノードのメモリにメタデータが格納されている
- データ分散

### マテリアライズドビュー

- SQLのViewのような機能
- 頻繁に実行するパターンを結合・フィルタ・集計・射影などを使って高速化

### 運用の自動化

- CloudWatchとの連動
- 自動バックアップ
  - スケジューリングやスナップショットが可能
- 自動メンテナンス
  - パッチ適用は自動で実施
  - 時間を指定できる
- スケジューリング機能
  - クラスターサイズの変更設定
  - 一時停止と再開時間を設定

### 機械学習によるクエリの効率化

- テーブルメンテナンスの自動化
  - テーブル分散スタイルの自動最適化
  - 統計情報を使ってデータを再編成する
- 自動ワークロード管理
  - 複数クエリの実行をワークロードで管理する際に機械学習でクエリ実行の優先順位を決めて自動化
- ショートアクセラレーション
  - クエリの実行時間を予測し、実行時間が短いクエリを優先して実行
  - WLM(work load management)キューを削減可能
- 設定のレコメンデーション
  - パフォーマンスを分析し最適化やコスト削減についてレコメンデーションする

### ワークロード管理(WLM)

- ワークロードに応じてキューを設定し、スロットでCPUやメモリを割り当てる。
- スロットを増やすと並列度が向上するが割り当てメモリは減少する。
- 説明がよくわからんよ？

### クエリエディタ

- マネコンでクエリを実行できる。

### スケーリング

- ノードを最大32個まで追加できる。ノードタイプの変更も可能
- クラスターを追加することも可能
- クラスタは自動的に数秒で追加でき、高速なパフォーマンスを維持
- 追加クラスタ数は1～10

### Redshift Spectrum

- S3に対して直接データ解析を可能。
- ユーザー管理のS3バケットを指定して、解析することができる。
- データをS3にため込む際にデータレイクとして使っていることが前提の機能
- AthenaやS3クエリとの相違点はこの部分。
- コンピュートノードの先にクエリエンジンが存在するような構成

### データ連携(To Redshift)

- S3
  - 最も頻繁に使われるデータ連携先
  - S3からデータを取得してRedshiftで解析することも可能
  - S3内部のデータ解析を直接実行することも可能
- Kinesis
  - Kinesis data Firehose を利用してストリーミングデータの格納先として Redshift を指定可能
  - これを解析に利用することが可能
- RDS
  - RDS との直接接続はない
  - AWS Data Pipeline や DMSを利用してデータ移行を実施可能
- DynamoDB
  - DynamoDB から Redshift にデータコピーを実行可能
- Amazon EMR
  - EMR から Redshift にデータコピーを実行可能

### データ連携(From Redshift)

- QuickSight
  - Redshift に接続して、データの可視化を実施可能
- S3
  - UNLOAD コマンドを実行することで、 Redshift から S3 へとデータを抽出することが可能
- Amazon Machine Learning
  - RedShift を機械学習の学習データとして設定して利用可能
- RDS
  - 直接に連携はできないが、 PostgreSQL の機能を利用してデータを RedShift から RDS と連携可能

### 関連機能

- QuickSight
  - データ可視化するBIツール
- AWS Glue
  - ETLを行うマネージド型サービス
  - Athena, Redshift, EMRにloadする
- AWS Lake Formation
  - データレイクの構成をベストプラクティスの中から構成を素早く実現することができる
- Amazon EMR
  - Spark, Hive, Prestoなどのビッグデータフレームワークを使用して大量データを処理・分析する。

### リザーブドノード

- オンデマンドではなく、リザーブドで購入することで安価にすることができる。

## 実際の作成

### 設定

- ノードのタイプの選択
- クラスターのIAMロール設定
- VPC、サブネットグループ、セキュリティグループなど
- 拡張されたVPCルーティング
- パブリックアクセス設定
- 暗号化
- メンテナンスウィンドウ
- CloudWatchによるモニタリング
- バックアップ設定

# ElastiCache

## 概要

- 分散インメモリキャッシュサービスの構築・管理および助リングを実施できる。
- 特徴
  - キャッシュクラスタを数クリックで起動
  - フルマネージド型でモニタリング、自動障害検出、復旧、拡張、パッチ適用、バックアップに対応し高可用性を実現
  - 広く利用されている2種類のエンジンmemcached, redisから選択可能

### Redis

- より高機能。

- 高速に値をRead/Writeできるインメモリキャッシュ型DB
- シングルスレッドで動作するインメモリキャッシュDBで全てのデータ操作は排他的
- スナップショット機能がある
- データを永続化できる
- 以下が必要な場合
  - 複雑なデータ型
  - リードレプリカ
  - pub/sub
  - フェイルオーバー
  - キーストアの永続性
  - バックアップと復元
  - 複数のデータベース

### Memcached

- よりシンプル。一時的なデータ置き場として使う。

- 高速に値をRead/Writeできるインモリキャッシュ型DB
- マルチスレッドで動作するインメモリキャッシュDB
- スナップショット機能がない
- データを永続化できない
- フェイルオーバーや復元ができない。
- 以下が必要な場合
  - シンプルなデータ型を使う
  - 複数コアまたはスレッドをもつ大きなノードを動かす必要がある
  - 需要に応じたスケールアウト・イン機能
  - オブジェクトをキャッシュする必要がある

### ElastiCache with Redis

- Luaスクリプト
- 位置情報クエリ
- pub/subモデル
  - チャットアプリなどのメッセージ処理、イベント処理

### ユースケース

- 用途
  - セッション管理
  - IOT 処理とストリーム分析
  - メタデータ蓄積
  - ソーシャルメディアのデータ処理／分析
  - Pub/Sub 処理
  - DB キャッシュ処理

- 構成
  - アクセス頻度のtら会データをキャッシュに配置する。
  - 通常のDBと組み合わせる

- 具体例
  - ユーザーのマッチング処理
  - レコメンデーションの結果処理
  - 画像データの高速表示
  - ゲームイベント終了時のランキング表示