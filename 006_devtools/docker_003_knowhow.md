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

## composeでnvidia-dockerを使う

- composeのversionが2の場合

```yml
version: '2.4'
services:
  sample:
    image: nvidia/cuda:11.2.1-base
    command: nvidia-smi
    runtime: nvidia
    environment:
      NVIDIA_VISIBLE_DEVICES: all
      NVIDIA_DRIVER_CAPABILITIES: utility,compute,video
```

- composeのversionが3の場合
  - deploy配下が必要な部分

```yml
version: '3.8'
services:
  sample:
    image: nvidia/cuda:11.2.1-base
    command: nvidia-smi
    deploy:
      resources:
        reservations:
          devices:
           - driver: nvidia
             capabilities: [utility, compute, video]
```

- 以下を参考
  - https://qiita.com/routerman/items/c5f9d7b6d03e44de6be2

## WSL2がdocker prune済みの領域を返してくれない

```ps
// WSLを止める
> wsl --shutdown

// diskpartを起動する（diskpartのウィンドウが開きます）
> diskpart

// 対象のvhdxファイルを指定（PATHは各自確認して下さい）
DISKPART> select vdisk file="C:\Users\[ユーザ名]\AppData\Local\Docker\wsl\data\ext4.vhdx"
DISKPART> attach vdisk readonly

// 最適化する
DISKPART> compact vdisk

// 終了
DISKPART> detach vdisk
DISKPART> exit
```

- 参考
  - https://7me.nobiki.com/2020/10/12/wsl2-docker-re-use-disk-space/

## GPGエラー

- apt updateでGPG エラーが出たら
  - https://qiita.com/HS310164/items/15ebb375726ff4db4eb2

## ML向けdockerの工夫点

- 機械学習なdockerfileを書くときに気をつけとくと良いこと
  - https://nykergoto.hatenablog.jp/entry/2020/07/25/%E6%A9%9F%E6%A2%B0%E5%AD%A6%E7%BF%92%E3%81%AAdockerfile%E3%82%92%E6%9B%B8%E3%81%8F%E3%81%A8%E3%81%8D%E3%81%AB%E6%B0%97%E3%82%92%E3%81%A4%E3%81%91%E3%81%A8%E3%81%8F%E3%81%A8%E8%89%AF%E3%81%84%E3%81%93


## 複数サービスの実行

- 一つのスクリプトにまとめてそれを実行するのが推奨の方法。
  - - https://docs.docker.jp/v19.03/config/container/multi-service_container.html

- daemonで動かすために、通常のsystemctlコマンドはコンテナ内で普通使えない。
  - 一応使う方法はあるが、今度は通常通りには使えない。
    - https://qiita.com/flour/items/977abe462955e4285d0b


## 一般ユーザーをコンテナ内に作成する

```
RUN apt-get install -y sudo
RUN useradd -m ubuntu
RUN echo 'ubuntu:ubuntu' | chpasswd
RUN usermod -aG sudo ubuntu
```

- 参考
  - https://qiita.com/nishina555/items/52aef60cfb82fb794b18


## SSH接続できるコンテナ環境の作成

- https://qiita.com/takat0-h0rikosh1/items/ae6d951a4b25d260894c


## コンテナ内から別コンテナに通信する

- 公式
  - https://docs.docker.jp/compose/networking.html#compose-links

- `コンテナサービス名:コンテナ側のポート`でアクセスできる。

- ホスト側のポートだと思ってたので要注意。
