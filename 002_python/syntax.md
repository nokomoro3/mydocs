# syntax(文法)

## リスト内包表記とif

- 普通のリスト内包表記
```python
sample_list = [0,1,2,3,4,5,6]
sample_list_2 = [ i*2 for i in sample_list]
sample_list_2
# OUT: [0, 2, 4, 6, 8, 10, 12]
```

- if/elseとの組み合わせ（配列サイズ不変）
```python
sample_list = [0,1,2,3,4,5,6]
sample_list_3 = [ i*2 if i%2==1 else 0 for i in sample_list]
sample_list_3
# OUT: [0, 2, 0, 6, 0, 10, 0]
```

- 抽出
```python
sample_list = [0,1,2,3,4,5,6]
sample_list_4 = [ i*2 for i in sample_list if i%2==1]
sample_list_4
# OUT: [2, 6, 10]
```
