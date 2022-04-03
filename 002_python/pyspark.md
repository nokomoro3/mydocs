# pyspark

## diff

既存のPandasのコードをSparkに置き換えたいと考えています。
下記のPandasのコードでは、Dataframeをソートした後、前のレコードのodometer_kmカラムの差分を出して、odometer_diffカラムを追加します。
pyspark.pandas.DataFrame.diffを使わずに、なんとかやりたいと考えています。

```python
df_normal = df_normal.sort_values(by=['id','record_time_round'])
df_normal['odometer_diff'] = df_normal.groupby(['id'])['odometer_km'].diff().fillna(0)
```

lead, lag関数はあるのでそれをうまく使うとできそうですが、:パンダの顔: のgroupbyからdiffの動きを脳内だけで妄想できてないので少々考えますー

ちょっと長くなっちゃったので処理を分割しましたが、一応こんな感じで同じ結果にはなりますー

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import lag, col
from pyspark.sql.window import Window

# ひとつ前の列を取得
df_normal = df_normal.withColumn(
    'odometer_diff',
    lag('odometer_km', 1).over(Window.partitionBy('id').orderBy('record_time_round'))
)
# diffを計算
df_normal = df_normal.withColumn(
    'odometer_diff',
    col('odometer_km') - col('odometer_diff')
)
# ゼロ埋め
df_normal = df_normal.na.fill(0)
# ソート
df_normal = df_normal.sort('id', 'record_time_round')
```

分割してないバージョンです

```
df_normal = df_normal.withColumn(
    'odometer_diff',
    col('odometer_km') -
    lag('odometer_km', 1).over(Window.partitionBy('id').orderBy('record_time_round'))
).na.fill(0).sort('id', 'record_time_round')
```

sparkのDataFrame自体はsqlとノリはあんま変わらないのでほとんどの関数はpandasのと1対1の翻訳になるのですが、今回のdiffみたいなシンタックスシュガー使ってるとシンドイのは盲点でした...