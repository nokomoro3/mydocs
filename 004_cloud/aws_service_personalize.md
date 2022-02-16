# AWS Personalize

## 公式

- https://docs.aws.amazon.com/personalize/latest/dg/what-is-personalize.html

## 概要

- Amazon Personalizeは、開発者がパーソナライズされた体験をユーザーに提供することを容易にする、フルマネージドな機械学習サービス。
- Amazonがパーソナライゼーションシステム構築で培った知識と経験が反映されている。
- 機械学習の経験がなくとも使える。

- Amazon Personalizeは以下のシナリオを準備する。
  - ユーザーの嗜好や行動に基づいたレコメンデーションの生成
  - 上記結果のパーソナライズされた再ランキング
  - メールなどの配信コンテンツのパーソナライズ
  - ユーザーセグメントに基づくターゲットマーケティングキャンペーンの作成
  
## 2種類のデータセットグループ

- Domain Dataset Group
  - あるドメインに最適化されたPersonalizeを使用するためのデータセットグループ。
  - 現時点では、以下の２種類のドメインが準備されている。
    - VIDEO_ON_DEMAND
    - ECOMMERCE
  - 下記の、Custom Usecaseで作成したソリューションを追加することも可能。

- Custom Dataset Group
  - カスタムリソースを用いてPersonalizeを実施したい場合に使用するデータセットグループ。
  - こちらではレシピを選択し、モデルをトレーニングする。
  - あとでDomainと関連付けることはできない。

## リアルタイム処理

- Amazon Personalizeはユーザーからのリアルタイムのイベントを取得し、リアルタイムのパーソナライゼーションを提供することが可能。
- Amazon Personalizeは、リアルタイムのユーザーアクティビティデータと、既存のユーザープロファイルおよびアイテム情報（履歴データ）をブレンドし、
ユーザーに最も関連性の高いアイテムを推奨することができます。
- また、Amazon Personalizeを使用して、新しいウェブサイトなどの新しいプロパティのインタラクションデータを収集し、
十分なデータが収集された後に、Amazon Personalizeがレコメンデーションを開始することができます。

## Domain Dataset Group

- ドメイン独自のスキーマが提供されるため、それに合わせてデータを準備する必要がある。
- ライフサイクルの管理は、Personalize側で実施してくれる。
- 最適なトレーニング構成を探索したりする必要がありません。
- 以下の手順で使用する。
  - ドメイン必須のフィールドを含むスキーマを作成。
  - データをそのスキーマに合わせて整形し、S3にUploadする。
  - データをimportし、ユーザーのインタラクションをリアルタイムに記録します。
  - 十分なデータが集まればユースケース毎にレコメンダーを作成します。
    - 満たすべき最低限のデータ要件は以下です。
      - 1000レコードのインタラクションデータ。
      - それぞれ2つ以上のインタラクションがある、25名以上のユニークなユーザー。
  - アプリケーションから、レコメンデーションを取得する。
  - カタログが大きくなっても、bulkまたはincremental data import operationでデータを最新に保つことが可能。
    - データセットに変更が生じても、Personalizeがレコメンダーのライフサイクルや更新を管理します。

## Custom Dataset Group

- ユースケースに合わせたトレーニングが可能。
- 以下のようなことが可能。
  - ユーザーのパーソナライゼーション
  - 関連するアイテムの提示
  - アイテムの再ランキング
  - ユーザーのセグメンテーション
- ユースケースに応じたレシピを選択してデータを入力します。
- Personalizeは特徴量抽出を行い、HPOを行って選択した学習アルゴリズムを使用します。

- レシピの中には、ユーザーのブラウジングコンテキストに基づいてレコメンデーションを提供できるものがあります。
  - たとえば、User-Personalizationレシピを使用すると、ユーザーがモバイルデバイスでブラウジングしているときと、
    同じユーザーがデスクトップでブラウジングしているときとで、Amazon Personalizeが異なるレコメンデーションを提供することができます。
  - どのレシピを使用するかを決定するために、Amazon Personalizeは、トレーニングされたソリューションバージョンを特徴付けるためのメトリックを提供します。

- 以下の手順で使用する。
  - カスタムスキーマの要件に基づいてデータを整形し、S3にuploadする。
  - データをimportし、ユーザーのインタラクションをリアルタイムに記録します。
  - レシピを選択し、十分なデータが集まればユースケース毎にレコメンダーを作成します。
    - 満たすべき最低限のデータ要件は以下です。
      - 1000レコードのインタラクションデータ。
      - それぞれ2つ以上のインタラクションがある、25名以上のユニークなユーザー。
    - レシピにより、必要なデータが異なります。
  - アプリケーションから、レコメンデーションやセグメンテーションを取得する。
    - リアルタイム推論(キャンペーン)
    - バッチジョブ
    - ユーザーセグメントジョブ

## Pricing

- Personalizeは従量課金です。
- 以下によりコストが変動します。
  - データの処理と保存
  - トレーニング
  - レコメンデーションの数

- 課金は、domain optimized recommenders、user segmentation、Custom solutionかで異なった体系となります。

- https://aws.amazon.com/personalize/pricing/?nc1=h_ls


## VIDEO_ON_DEMANDのスキーマ

- interactions dataset
  - 必須
    - USER_ID (string)
    - ITEM_ID (string)
    - TIMESTAMP (long)
    - EVENT_TYPE (string and depending on use case, Watch and Click event types)
      - ユースケースに依存した文字列。実際には、WatchとClickが対応。
        - https://docs.aws.amazon.com/personalize/latest/dg/interactions-datasets.html#event-type-and-event-value-data
  - 予約
    - EVENT_VALUE (float, null)
      - 動画の視聴率(動画を全体のどこまで見たか、など)
      - training時のフィルタリングに使用される。
        - https://docs.aws.amazon.com/personalize/latest/dg/interactions-datasets.html#event-type-and-event-value-data
    - IMPRESSION (string)
      - イベント時(interaction時)にユーザーに示されたアイテムのリスト。
      - impressionには暗黙的(implicit)、明示的(explicit)の２パターンがある。
        - implicitはPutEvent時にrecommendationIdパラメータを指定し、そのrecommendationIdのimpressionを自動で取得する。
        - explicitはPutEvent時にimpressionパラメータを|区切りで指定する。例えば在庫ありのみにimpressionを限定したい場合などに使用する。
        - アイテムの順番は意味をなさない。
        - 双方指定した場合は、explicit impressionsを使用する。
    - RECOMMENDATION_ID (string, null)
      - 説明がないが、implicit impressionにはこちらを使うという意味だろう。

- Users dataset
  - 必須
    - USER_ID (string)
    - 1 metadata field (categorical string or numerical)
    - categoricalの場合、以下のようにschema定義が必要。
    ```json
    {
      "type": "record",
      "name": "Users",
      "namespace": "com.amazonaws.personalize.schema",
      "fields": [
          {
              "name": "USER_ID",
              "type": "string"
          },
          {
              "name": "SUBSCRIPTION_MODEL",
              "type": "string",
              "categorical": true
          }
      ],
      "version": "1.0"
    }
    ```
- Items dataset
  - 必須
    - ITEM_ID
    - GENRES (categorical string)
    - CREATION_TIMESTAMP (in Unix epoch time format)
  - 予約
    - PRICE (float)
    - DURATION (float)
    - GENRE_L2 (categorical string)
    - GENRE_L3 (categorical string)
    - AVERAGE_RATING (float)
    - PRODUCT_DESCRIPTION (textual)
    - CONTENT_OWNER (categorical string)
    - CONTENT_CLASSIFICATION (categorical string)
  - GENREについては、複数のGENREや階層のGENREを付与できる。
    - https://docs.aws.amazon.com/personalize/latest/dg/VIDEO-ON-DEMAND-items-dataset.html#VIDEO-ON-DEMAND-items-categorical-data

## VIDEO_ONDEMANDのuse cases(recommender)

- https://docs.aws.amazon.com/personalize/latest/dg/VIDEO_ON_DEMAND-use-cases.html

- 4つのuse cases(recommender)があり、それぞれで必要なデータが変わる。

- Most popular
  - Watchイベントタイプに基づいて、各アイテムのWatch総数に基づいて、popularityを計算して提示する。
  - GetRecommendations APIで結果を取得し、userIdがパラメータとして必要。
  - trainingには、Watch Eventのみが使用される。
- Because you watched X
  - データセットの共起に基づいて、あるアイテムを見た場合のおすすめの類似したアイテムを取得する。
  - GetRecommendations APIで結果を取得し、userId、itemIdがパラメータとして必要。
  - userIdにより、視聴済みのデータはフィルタリングされる。
  - trainingには、Watch Eventのみが使用される。
- More like X
  - 指定した動画と類似したアイテムを取得する。
  - GetRecommendations APIで結果を取得し、userId、itemIdがパラメータとして必要。
  - userIdにより、視聴済みのデータはフィルタリングされる。
  - trainingには、Watch, Click Eventが使用される。(Clickイベントは必須ではない)
- Top picks for you
  - ユーザーに推薦したいアイテムを取得する。
  - GetRecommendations APIで結果を取得し、userIdがパラメータとして必要。
  - userIdにより、視聴済みのデータはフィルタリングされる。
  - trainingには、Watch, Click Eventが使用される。(Clickイベントは必須ではない)

- recommenderにはconfigurationがあり、共通の設定項目と個別の設定がある。
  - 共通
    - Minimum recommendation requests per second
      - 秒間あたりに処理するリクエストを決定する。
      - ここを調整することでコストに直接的には影響しないが、recommendationが増えるとコストが増えるので注意。

  - Top picks for youのみ
    - Emphasis on exploring less relevent items
      - 関連性の低いアイテムに重点的に探索する。1に近いほど探索を広げ、0だと探索しない。デフォルトは0.3。
    - Exploration item age cut off
      - 探索するinteractionの期間を指定する。デフォルトは30days。


## VIDEO_ONDEMANDのsolutions

- Custom dataset groupと同様、solutionを作成できる。加えて、training時のオプションをいろいろと指定可能。

- event_typeやevent_valueでフィルタした結果を学習することができる。

- 別の目的関数とする列を指定することで、レコメンド以外のソリューション提供が可能。例としては収益の最大化など。実施にはitem datasetが必要。
  - これは割とアツい気がする。
  - これは Custom dataset group でも（前から）あったみたい。

- recipesは、Custom dataset group と同じようだ。
  - https://docs.aws.amazon.com/ja_jp/personalize/latest/dg/working-with-predefined-recipes.html

## 各種サービスのquotas

- https://docs.aws.amazon.com/personalize/latest/dg/limits.html#limits-table

## 検証が必要なアップデート

- https://aws.amazon.com/blogs/machine-learning/amazon-personalize-announces-recommenders-optimized-for-retail-and-media-entertainment/

- https://aws.amazon.com/blogs/machine-learning/improve-the-return-on-your-marketing-investments-with-intelligent-user-segmentation-in-amazon-personalize/