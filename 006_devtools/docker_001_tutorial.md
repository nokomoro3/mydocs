# Dockerチュートリアル


## Dockerインストール方法

### Windows

- WSLをインストールする。
  - 以下のリンクの手順を１～５まで実施する。
    - https://docs.microsoft.com/ja-jp/windows/wsl/install-manual

- Docker Desktop for Windowsを以下のリンクからダウンロードする。
  - https://www.docker.com/products/docker-desktop

- ダウンロードされたexeファイルをインストールする。

- PowerShellかコマンドプロンプトから、Hello WorldできればOK。
```powershell
docker run hello-world
```

### Mac

- Docker Desktopを以下のリンクからダウンロードする。
  - https://www.docker.com/products/docker-desktop

- ダウンロードされたdmgファイルをインストールする。
  - dmgファイルのインストール方法はこちらを参照。
    - http://lee-ways.com/macdmg/

- ターミナルを起動し、Hello WorldできればOK。
```sh
docker run hello-world
```

- Macの人は、ターミナルのセキュリティがデフォルトでは厳しいので、<br>以下の通りにシステム環境設定で「ターミナル」アプリにフルディスクアクセスを許可する。
  - https://qiita.com/KEINOS/items/0366f1c281b574a79cfb

### Linux(Ubuntu)

- 以下に従いインストールする。
  - https://docs.docker.com/engine/install/ubuntu/


## Docker基本的なコマンド

- pull: イメージをダウンロードする。
  - TAGはバージョンみたいなもの。
```sh
# docker pull <REPOSITORY>:<TAG>
docker pull ubuntu:latest # 一例、TAGがない場合はlatestが取得される。
```

- images: ダウンロード済みのイメージ一覧を見る。
  - 最初に落としたHelloWorldもある。
```sh
$ docker images
REPOSITORY   TAG       IMAGE ID       CREATED       SIZE
hello-world  latest    feb5d9fea6a5   7 weeks ago   13.3kB
ubuntu       latest    ba6acccedd29   3 weeks ago   72.8MB
```

- run: イメージからコンテナをつくる。
```sh
# docker run -itd --name <CONTAINER_NAME> <REPOSITORY>:<TAG> bash
docker run -itd --name sample ubuntu:latest bash # 一例
```

- ps: 作成済みのコンテナ一覧を見る。
```sh
$ docker ps -a # 停止中のものを含めてみたい場合
$ docker ps # running中のものだけ見たい場合
```

- exec: コンテナ内にログインする。
```sh
# docker exec -it <CONTAINER_NAME> bash
docker exec -it sample bash
```

- コンテナからログアウトする。
```sh
exit
```

- logs: 稼働中のコンテナのログを見る。
```sh
# docker logs <CONTAINER_NAME>
docker logs sample
```

- inspect: 稼働中のコンテナの設定を確認する。
```sh
# docker inspect <CONTAINER_NAME>
docker inspect sample
```

- stop: コンテナを止める(止めるだけで消えない)。
```sh
# docker stop <CONTAINER_NAME>
docker stop sample
```

- rm: コンテナを消す。
```sh
# docker rm <CONTAINER_NAME>
docker rm sample
```

- rmi: イメージを消す。
```sh
# docker rmi <REPOSITORY>:<TAG>
docker rmi ubuntu:latest
```

- prune: 未使用のコンテナやイメージをまとめて消す。
```sh
docker image prune
docker container prune
```
