# PyTorch

## 作り方

### Datasetの作成

#### 作成
- `torch.utils.data.Dataset`を継承したDatasetクラスを作成する。
- 実装が必要な関数は３つ。

```py
class MyDataset(torch.utils.data.Dataset):
    def __init__(self):
        pass
    def __getitem__(self, idx):
        # ...
        return x, y # xは入力、yは出力(正解)。
    def __len__(self):
        return len(...)

#  動作確認用
BATCH_SIZE = 128
ds_train = MyDataset()
dl_train = DataLoader(ds_train, BATCH_SIZE, shuffle=True)
for feats, targets in dl_train:
    print(feats, targets)
    break
```

#### トラブルシューティング
- 以下のエラーが出る場合、__getitem__の戻り値を確認する。
```
TypeError: default_collate: batch must contain tensors, numpy arrays, numbers, dicts or lists; found object
```

- numpy配列を戻している場合でも、dtypeがobjectの場合、上記のエラーがでた。
- astypeでnp.float64などにキャストが必要

## 基本演算

## squeeze

- たとえば、(A×1×B×C×1×D)を(A×B×C×D)に変換できる。

- 参考
  - https://pytorch.org/docs/stable/generated/torch.squeeze.html

## CUDAモードの確認

- https://qiita.com/Haaamaaaaa/items/20c0dc16c2affab37fa5