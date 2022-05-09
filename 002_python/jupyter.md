# jupyter

## 起動コマンド

- localhostアクセスする場合に必要な最低限のコマンド

```shell
jupyter lab --port=8888 --ip=0.0.0.0 --allow-root --NotebookApp.token=''
```

## markdownの表を全体左に寄せる。

- 冒頭に以下のhtmlを書く。
```
%%html
<style>
table {float:left}
</style>
```

## コンソールコマンドに変数を渡す方法

- https://qiita.com/piruty/items/9c4f87cc2240e1028b31

