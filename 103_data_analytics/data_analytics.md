# data analytics

## PySpark

- PySparkのページ
  - https://x1.inkenkun.com/archives/category/spark

## データ・オペレーションとは

- https://lab.astamuse.co.jp/entry/data-operation

## Data mesh

- data meshとは何か

  - https://martinfowler.com/articles/data-mesh-principles.html

## EAIとETLの混同について

- EAIは各業務アプリの統合する概念
- EAIは廃れたが、マイクロサービスに思想が残った。
- EAIに求められるもの・性質
  - リアルタイム、イベントベース、データが少ない単位、たくさんのプロトコル、１対多(Pub/Sub)
- 以上のことから、データ分析基盤とは全く異なる。（データレイクとも）
  - そもそもデータの持ち方が違う。EAIは中央集権的なDBを持たない。蓄積しない。
  - ちなみにそれは、データ分析基盤のDWHとも違う。
- アステリアワーク、データスパイダーはDWHだが、短い間隔でETLすることで、EAIを実現しようとして失敗してしまうことがある。
  - AWSでイベントベースにEAIを実現しようとすると、kinesisを使うことになる。
  - DWH製品は主キー制約が使えない、用途によって道具は変えないといけない。

## DBT

- DBTまとめ
  - https://github.com/Hiflylabs/awesome-dbt#cicd

- DBT活用
  - https://dev.classmethod.jp/articles/hajime-ippo-dbt-20220310/