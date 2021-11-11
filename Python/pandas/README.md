# Pandas

## 列を並び変える。

- 並び変えたカラムを[]に入れるだけで良い。

```python
columns = df.columns.tolist()
columns = columns[-1:] + columns[:-1] # reorder example
df = df[columns] # do reorder!!
```