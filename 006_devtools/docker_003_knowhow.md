# Dockerノウハウ

## ビルド時に特定のRUN以降のキャッシュを無効にする。

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

## コンテナを起動したままにする方法。

- 以下で起動状態を維持できる。
```sh
$ docker run -itd {イメージ名}:{タグ} /bin/sh
$ docker exec -it {イメージ名}:{タグ} /bin/sh # その後、これでログイン
```

- docker-composeで、tty:trueとしてもできる。
```yml
version: '2'
services:
  sample:
    image: {イメージ名}:{タグ名}
    container_name: sample_container
    tty: true
```

- 参考
  - Dockerのコンテナを起動したままにする
    - https://qiita.com/reflet/items/dd65f9ffef2a2fcfcf30

## コンテナの自動起動設定

- docker-composeの場合、restart: alwaysを指定する。
```yml
services:
  ubuntu:
    image: ubuntu:latest
    restart: always
```

- docker runコマンド実行時に以下の引数を与えることで、自動起動にできる。
```sh
$ docker run --restart=always ...
```

- 既に起動済みのコンテナにリスタートする用に設定を変更する。
```sh
$ docker update --restart=always <CONTAINER NAME>
```

- container一覧の自動起動設定を取得する。
```sh
$ docker inspect -f "{{.Name}} {{.HostConfig.RestartPolicy.Name}}" $(docker ps -aq)
```

- 参考
  - https://www.pressmantech.com/tech/6522

## composeでビルドする際に、image名を指定する

- imageというパラメータを使えばよい。階層は、buildと同じ。
```yml
version: '2.3'
services:
  sample_service:
    build:
      context: .
      dockerfile: Dockerfile
      image: sample:latest
```

- 参考
  - https://amaya382.hatenablog.jp/entry/2017/04/03/034002