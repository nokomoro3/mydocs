# Docker on WSL2

## WSL2(Ubuntu 20.04 LTS)でDockerを動かす。

- 注意点
  - そもそも色々問題があるのでおすすめしない。
  - 事前準備として、Docker desktop for Windowsは必要。

- docker CLIインストール
  - 以下に沿ってinstallするが、最後のapt-getはdocker-ce-cliのみでOK。
    - https://docs.docker.com/engine/install/ubuntu/

- docker desktopで以下を設定。

<img width="900" src="./fig001.drawio.svg">

- WSL一覧を確認。2になっているか確認する。
```powershell
> wsl --list --verbose
* Ubuntu-20.04           Running         1
  docker-desktop         Stopped         2
  docker-desktop-data    Stopped         2
```

- WSLのdefaultを一応、Ubuntu 20.04 LTSにしておく。
```powershell
> wsl --set-default Ubuntu-20.04
```

- docker desktopをrestartする。

- WSLも以下のコマンドで再起動する。
```powershell
> wsl --shutdown Ubuntu-20.04
```

- hello-worldでテスト。
```sh
$ docker run hello-world
```

## 注意

- 以下を実施するとdockerデーモンにアクセスできなくなるので、設定しない。
  - port2375を有効化
    - Docker Desktopのメニューで、以下にチェックをする。
      - 「Expose daemon on tcp://localhost:2375 without TLS」
  - 環境変数を設定
    - 以下で設定
    ```sh
    $ echo "export DOCKER_HOST=tcp://localhost:2375" >~/.bashrc
    ```

## 参考：WSL2でnvidia-dockerも動かす方法

- うまくいかなかったが、以下の手順に沿ってやればできる人もいるらしい。
  - https://docs.nvidia.com/cuda/wsl-user-guide/index.html

- Windows 11だともっと簡単なのかもしれない。
