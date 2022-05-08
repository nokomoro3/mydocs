# sklearn

## pipeline

- sklearnのpipelineに自分で定義した関数を流し込む - Qiita
  - https://qiita.com/kazetof/items/fcfabfc3d737a8ff8668

## data

- train_test_splitのindex番号を記憶する方法
  - 引数`indices`にindex番号を指定すればOK

```python
X_train, X_test, Y_train, Y_test, indices_train, indices_test = train_test_split(X, Y, indices, test_size=0.33)
```

- 参考
  - https://qiita.com/haru1977/items/7aaf1eaeb0747fe613be
