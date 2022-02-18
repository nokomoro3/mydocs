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

- recipe type: USER_PERSONALIZATION
  - recipe: Popularity-Count
    - interactionデータから、最も人気のアイテムをレコメンドする。
    - このレシピは、すべてのユーザーに対して同じ popular items を returnする。
    - このレシピはMetricsの比較のベースラインとなります。

  - recipe: HRNN(legacy)
    - このレシピは、User-Personalizationレシピに改善および統合されているため、そちらの使用を推奨します。
    - HRNNとはHierachical RNNの略です。
    - 学習のHyper parameterとしては以下が準備されています。
      - hidden_dimension(HPO tunable: Yes)
        - 隠れノード数です。より複雑なモデル化が可能になります。
      - bptt: back propagation through time(HPO tunable: Yes)
        - どれだけ過去の情報を使って推論をするか設定できます。
      - recency_mask(HPO tunable: Yes)
        - 直近の傾向をモデルに反映するか否かを設定できます。
        - Trueの場合、直近の傾向を重視して学習します。
      - min_user_history_length_percentile, max_user_history_length_percentile(HPO tunable: No)
        - 履歴の短い・長いユーザーを学習データから削除するパラメータです。
        - 短い・長いの判断は相対的なパーセントで指定します。

  - recipe: HRNN-Metadata(legacy)
    - HRNN レシピに、metadataから推測される特徴を考慮したものです。
    - metadataは、Interactions, Users, Itemsそれぞれに含まれるデータです。
    - metadataを使うことでより正確なレコメンドを学習できる可能性があります。
    - ただし学習にかかる時間は大きくなります。
    - 学習のHyper parameterは、HRNNレシピと同一です。

  - recipe: HRNN-Coldstart(legacy)
    - 新しいアイテムやインタラクションを頻繁に追加し、それらをレコメンデーション結果にすぐ反映したい場合のレシピ。
    - metadataレシピに類似していますが、より新しいアイテムのレコメンデーションに強い。
    - 下記のいずれかを満たすitemをcold itemとして扱う。
      - interaction数が一定数(cold_start_max_interactions )以下のitem
      - interactionの継続時間が一定期間以内のitem
        - より具体的には、cold_start_relative_from から cold_start_max_duration(日数、ただしfloat指定可) の範囲で指定する。
    - 履歴がある程度たまった後では、意味がないニュース配信などは、このコールドスタート問題がより深刻である。
    - 学習のHyper parameterは、HRNNレシピに加えて以下が設定できます。詳細は上述の通り。
      - cold_start_max_interactions(HPO tunable: No)
      - cold_start_max_duration(HPO tunable: No)
      - cold_start_relative_from(HPO tunable: No)

  - recipe: User-Personalization
    - Legacy Recipeが統合され、最適なソリューションを学習できます。
    - バックグラウンドで２時間毎に最新のモデルに自動更新をします。
    - 自動更新は完全なフル再学習ではないため、以前として手動で毎週学習が必要です。(その際、TrainingModeをFULLにします)
    - ２時間毎の頻度では少ない場合、手動でTrainingMode=UPDATEにして学習します。
    - 一度手動で学習した場合、自動更新はされなくなるため注意が必要です。
    - Note: この自動アップデートは費用がかかりません。
    - Legacyの変更点として、IMPRESSIONデータが使用できます。
    - 学習のHyperparameterとしては以下があります。一部、Legacyレシピと同じものがあります。
      - hidden_dimension(HPO tunable: Yes)
      - bptt: back propagation through time(HPO tunable: Yes)
      - recency_mask(HPO tunable: Yes)
      - min_user_history_length_percentile, max_user_history_length_percentile(HPO tunable: No)
      - exploration_weight(HPO tunable: No)
        - 関連性の低いアイテムに重点的に探索する。1に近いほど探索を広げ、0だと探索しない。デフォルトは0.3。
        - Recommendationにも同様のものがある。
      - exploration_item_age_cut_off(HPO tunable: No)
        - 探索するinteractionの期間を指定する。デフォルトは30days。
        - Recommendationにも同様のものがある。
- recipe type: PERSONALIZED_RANKING
  - recipe: Personalized-Ranking
    - ITEM_IDのリストをリクエストすると、ユーザーに応じたランキング順を取得できる。
    - 存在しないITEM_IDも処理可能だが、取得結果の最後尾に配置される。
    - 学習のパラメータは、HRNN(legacy)と同一である。

- recipe type: RELATED_ITEMS
  - recipe: Similar-Items
    - interactionと、itemの非構造化データを含むメタデータを使ってアイテムの類似度を計算します。
    - 異なるユーザーが既に見た、似た説明のあるアイテムを推奨することができます。
    - itemのメタデータがない場合に、似たアイテムをレコメンドしたい場合は、SIMS recipeを使用してください。
    - 学習のHyperparameterとしては以下があります。
      - item_id_hidden_dimension(HPO tunable: Yes)
        - interactionから計算するモデルの隠れ層のノード数。より複雑なモデル化が可能になります。
      - item_metadata_hidden_dimension(HPO tunable: Yes)
        - itemのmetadataから計算するモデルの隠れ層のノード数。より複雑なモデル化が可能になります。
      
  - recipe: SIMS
    - interactionデータを使ってアイテムの類似度を計算します。
    - 学習のHyperparameterとしては以下があります。
      - popularity_discount_factor(HPO tunable: Yes)
        - 人気度合いとitem間のinteractionの相関性どちらを優先するかのパラメータ。
        - 1.0であれば、相関性を無視し、0.0であれば人気度合いを無視する。
        - 通常0.5の設定が推奨である。
      - min_cointeraction_count(HPO tunable: Yes)
        - 類似性を計算するために必要な、最小のco-interaction数。
        - 3であれば、相関を計算する対象のそれぞれにinteractionしたユーザーが3人以上必要、という意味となります。
      - min_user_history_length_percentile, max_user_history_length_percentile(HPO tunable: No)
        - HRNN(legacy) recipeと同じパラメータです。
        - 履歴の短い・長いユーザーを学習データから削除するパラメータです。
        - 短い・長いの判断は相対的なパーセントで指定します。
      - min_item_interaction_count_percentile, max_item_interaction_count_percentile(HPO tunable: No)
        - interaction数の少ない、多いitemを学習データから削除するパラメータです。
        - 少ない・多いの判断は相対的なパーセントで指定します。
- recipe type: USER_SEGMENTATION
  - recipe: Item-Affinity
    - ユーザーセグメンテーションは、バッチジョブでのみ処理できます。
    - 必要なデータは、interactionsのみであり、itemsとusersはオプションです。
    - interactionのitem_idに基づいて、ユーザーをセグメントに分けます。
    - 学習のHyperparameterとしては以下があり、HPOには対応していません。
      - hidden_dimension
        - interactionから計算するモデルの隠れ層のノード数。より複雑なモデル化が可能になります。
  - recipe: Item-Attribute-Affinity
    - ユーザーセグメンテーションは、バッチジョブでのみ処理できます。
    - 必要なデータは、interactionsとitemsとなります。
    - アイテムの属性に基づいて、ユーザーをセグメントに分けます。
    - 学習のHyperparameterとしては以下があり、HPOには対応していません。
      - hidden_dimension
        - interactionから計算するモデルの隠れ層のノード数。より複雑なモデル化が可能になります。

## AutoML

- AutoMLは、APIのみでの提供となっている。
  - https://docs.aws.amazon.com/personalize/latest/dg/legacy-user-personalization-recipes.html

## SDKの使い方

- training
  - https://docs.aws.amazon.com/personalize/latest/dg/native-recipe-new-item-USER_PERSONALIZATION.html#training-user-personalization-recipe

- inference
  - https://docs.aws.amazon.com/personalize/latest/dg/native-recipe-new-item-USER_PERSONALIZATION.html#user-personalization-get-recommendations-recording-impressions

## Metricsの使い方

- https://docs.aws.amazon.com/personalize/latest/dg/working-with-training-metrics.html

## Recording Events

- https://docs.aws.amazon.com/personalize/latest/dg/recording-events.html

## 各種サービスのquotas

- https://docs.aws.amazon.com/personalize/latest/dg/limits.html#limits-table

## 公式のアップデート

- [2020.08.17] USER_PERSONALIZATIONのリリース
  - https://aws.amazon.com/jp/blogs/machine-learning/amazon-personalize-can-now-create-up-to-50-better-recommendations-for-fast-changing-catalogs-of-new-products-and-fresh-content/

- [2021.11.29] Domain Dataset Groupのリリース
  - https://aws.amazon.com/blogs/machine-learning/amazon-personalize-announces-recommenders-optimized-for-retail-and-media-entertainment/

- [2021.11.29] User Segmentationのリリース
  - https://aws.amazon.com/blogs/machine-learning/improve-the-return-on-your-marketing-investments-with-intelligent-user-segmentation-in-amazon-personalize/

## 過去の参考記事

- [2019.07.07] Amazon Personalizeを使ってみた
  - https://dev.classmethod.jp/articles/yoshim-amazon-personalize-tutorial/
  - 用語の説明が詳しく、その後やってみたの記事となっている。
- [2019.12.07] Amazon Personalize でコールドアイテムに対応したレシピでレコメンドしてみた – 機械学習 on AWS Advent Calendar 2019
  - https://dev.classmethod.jp/articles/2019advent-calendar-ml-on-aws-1207
  - HRNN-coldstart recipeを試している。
- [2020.02.26]
  - https://dev.classmethod.jp/articles/personalize-untrained-user/
  - 未学習ユーザーに対する各レシピの挙動を調査している。(HRNN, HRNN-metadata, Popularity Count)
  - SDKからの実行例が多く記載されている。
- [2020.03.04] PersonalizeのAutoML機能を使ったレシピの自動選択を試してみた
  - https://dev.classmethod.jp/articles/personalize-automl/
  - AutoMLを試してみた記事。AutoMLは現在、APIからしか利用できなくなっている気がする。
- [2020.03.23] Amazon Personalizeでハイパーパラメータのチューニング結果を取得できるようになりました!
  - https://dev.classmethod.jp/articles/personalize-hpo-result/
  - HPO, AutoMLの結果が取得できるようになったという記事。
- [2020.06.10] Amazon Personalizeに選択したEventTypeのアイテムをレコメンドから除外するフィルターが追加されました
  - https://dev.classmethod.jp/articles/amazon-personalize-update-recommendation-filter/
  - EVENT_TYPEを使ったフィルタリングのトライアル。
  - 学習データに存在しないEVENT_TYPEがフィルタリングされない結果となっている。
- [2020.08.28] Amazon Personalizeの推論フィルターでアイテムやユーザーのメタデータを条件としてフィルタできるようになりました
  - https://dev.classmethod.jp/articles/amazon-personalize-update-enhancing-recommendation-filter/
  - itemsのmetadataを使ったフィルタリング
- [2020.08.31] Amazon Personalizeに新たなレシピ「USER_PERSONALIZATION」が追加されました
  - https://dev.classmethod.jp/articles/amazon-personalize-update-new-recipe-user-personalization/
  - HRNN, HRNN-metadata, HRNN-coldstartを統合・改善したレシピ、USER_PERSONALIZATIONのトライアル。
  - IMPRESSION、CREATION_TIMESTAMPの説明、HRNNとの比較実験や、trainingMode=UPDATEの説明などが含まれる。
- [2020.10.31] Amazon Personalizeの推論フィルターを使って年齢制限のある商品やコンテンツをユーザーの年齢に応じてフィルターする
  - https://dev.classmethod.jp/articles/amazon-personalize-recommendation-filter-age-limit/
  - より具体的なフィルター実例

## 基礎原理

- 協調フィルタリング
  - https://qiita.com/hik0107/items/96c483afd6fb2f077985

- バンディットアルゴリズムを用いた推薦システムの構成について
  - https://techblog.zozo.com/entry/zozoresearch-bandit-overviews