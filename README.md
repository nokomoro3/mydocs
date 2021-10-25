# mydocs

## 開発ツール

### Git

#### Windowsで複数のgitアカウントを使い分ける。

- Git for Windowsは必要。credential.namespaceを使えばできる。

- オプション付きでcloneする。
```
git -c credential.namespace=xxx clone https://github.com/[name]/[repository].git
```

- repositoryに移動し、gitのlocal configでnamespaceを設定する。
```
cd [repository]
git config --local credential.namespace xxx
```

- 参考
  - https://qiita.com/shiena/items/fc7783a82d59be5ff259


### Docker

#### 特定のRUN以降のキャッシュを無効にする。

- ARGを適当な場所に入れる。

```yml
ARG CACHEBUST=1
```

- build時に引数を設定する。
```
$ docker build -t tagname --build-arg CACHEBUST=$(date +%s) .
```


- 参考
  - https://www.chazine.com/archives/4038
