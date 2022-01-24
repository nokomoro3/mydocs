# Anaconda

## 仮想環境作成

```bash
conda create -n <ENV NAME> python=3.8
```

## 仮想環境削除

```bash
conda remove -n <ENV NAME> --all
```

## エクスポート

```bash
conda env export > environment.yaml
```

## インポート

```bash
conda env create -f environment.yaml
```

## レポジトリ有償化対策

- miniconda + conda-forgeを使う
  - https://qiita.com/kimisyo/items/986802ea52974b92df27

## 参考

- python / conda環境構築の基本コマンド一覧
  - https://qiita.com/yampy/items/4e89cd97179bbd20f726
