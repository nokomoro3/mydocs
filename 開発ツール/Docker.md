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


#### コンテナを起動したままにする方法。

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