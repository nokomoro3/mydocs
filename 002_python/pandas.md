# Pandas

## 列を並び変える。

- 並び変えたカラムを[]に入れるだけで良い。

```python
columns = df.columns.tolist()
columns = columns[-1:] + columns[:-1] # reorder example
df = df[columns] # do reorder!!
```

## group集計して統計量を計算する。

- 既に準備されている統計計算をする場合

```python
# user_idで集計して、年齢の統計量を計算する例
age_stats_df = rowdata_df.groupby("user_id")["age"].agg(["mean"])

# 上記では、goupbyしたカラムがindexになってしまうので、嫌な場合はreset_index()する。
age_stats_df = rowdata_df.groupby("user_id")["age"].agg(["mean"]).reset_index()
```

- 自作でpersentileなどを計算したい場合

```python
# 自作関数を作る
def percentile(n):
    def percentile_(x):
        return np.percentile(x, n)
    percentile_.__name__ = 'percentile_%s' % n
    return percentile_

age_stats_df = rowdata_df.groupby("user_id")["age"].agg([percentile(80)]).reset_index()
```

- 時系列データを集計する場合
  - 時刻情報がある場合、その列をindexにすることで、resampleが使えて集計できる。
  - 購入量の時系列データのイメージで以下は'M'で月ごとの結果を集計する例。
```python
timedomain_df.set_index("recorded_at", drop=True).sort_index().loc[:,["purchase_count"]].resample('M').agg(['sum', 'mean', "max"])
```

## 行を挿入する。

- seriesをappendする。

```python
import pandas as pd

df = pd.DataFrame([], columns=['A', 'B', 'C'])
row_data = pd.Series(['0', '1', '2'], index=df.columns)

df = df.append(row_data, ignore_index=True)
df = df.append(row_data, ignore_index=True)
```

- dictをappendする。
```python
df = df.append({'A': 0, 'B': 1, 'C': 2}, ignore_index=True))
df = df.append({'A': 0, 'B': 1, 'C': 2}, ignore_index=True))
```

## 列毎の欠損数を求める。

```py
df.isnull().sum()
```

## applyの使用法まとめ

- 以下ノートブック参照
  - [pandas_apply_examples.ipynb](./notebooks/pandas_apply_examples.ipynb)


## query記法を使った条件抽出

- カラム名に空白のある場合はバッククォートを使う。

```python
df_receipt.query('`sales ymd` == 20181103')
```