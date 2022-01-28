# seaborn

- 可視化ライブラリ。
- matplotlibよりもDataFrameとの親和性が高い。

## おまじない
```py
import matplotlib.pyplot as plt
import seaborn as sns
```

## プロットの全体styleを変える

- https://qiita.com/eriksoon/items/b93030ba4dc686ecfbba#pltstyleuseseaborn-whitegrid

## サイズの変更

- ほとんどは、matplotlibと同じ感じ。
```py
fig = plt.figure(figsize=(10, 5), dpi=100)
# その後、各種プロット
```

- ただし、以下にあるようにplotの種類によっては設定方法が違う。
  - https://qiita.com/nj_ryoo0/items/9105ddfdf1b08b58398e
  - pairplot、relplot、catplot、lmplot、PairGrid、FacetGridが該当。
```py
g = sns.lmplot(...)
g.fig.set_figheight(10)
g.fig.set_figwidth(20)
```

## タイトル付与

```py
p = sns.lineplot(...)
p.set_title("Title")
p.set_xlim([-1.0, 1.0])
p.set_ylim([-1.0, 1.0])
```

## 軸範囲設定

```py
p.set_xlim([-1.0, 1.0])
p.set_ylim([-1.0, 1.0])
```

## 散布図
```py
fig = plt.figure(figsize=(10, 5))
p = sns.scatterplot(x=x, y=y)
```

## 折れ線グラフ(lineplot)

```py
fig = plt.figure(figsize=(10, 5))
p = sns.lineplot(x=x, y=y)
```

## replot(relational-plot)
W.I.P.

## 累積度数分布(ECDF: Empirical Cumulative Distribution Function)
- "経験的"累積分布関数を描画する。
- DataFrameとecdfしたいカラム名を指定する。
```py
fig = plt.figure(figsize=(10, 5))
p = sns.ecdfplot(x='column name', data=df)
```

## カーネル密度推定(KDE: Kernel Density Estimate)
- 確率密度関数の推定を描画する。
  - ヒストグラムのようにbin数を悩まなくていいのが嬉しい。
- DataFrameとecdfしたいカラム名を指定する。
```py
fig = plt.figure(figsize=(10, 5))
p = sns.kdeplot(x='column name', data=df)
```

## heatmap
- https://seaborn.pydata.org/generated/seaborn.heatmap.html
```py
fig = plt.figure(figsize=(10, 5))
p = sns.heatmap(np_data, vmin=-20, vmax=20) # np_dataは行列で与える。
```

## subplot

```py
fig, ax = plt.subplots(2,2, figsize=(10, 5))
p = sns.ecdfplot(x='column name', data=df, ax=ax[0,0])
p = sns.kdeplot(x='column name', data=df, ax=ax[0,1])
```

## 参考
- https://qiita.com/kakiuchis/items/f7c830a2b726992a6165#histplot%E3%83%92%E3%82%B9%E3%83%88%E3%82%B0%E3%83%A9%E3%83%A0