# NVIDIAドライバ周り

## 推奨設定で更新する方法

- アンインストール
```sh
$ sudo apt-get --purge remove nvidia-*
$ sudo apt-get --purge remove cuda-*
```

- 無理に最新を入れると、古いdistributionで動かなかったため、推奨ドライバを以下で確認する。
```sh
$ ubuntu-drivers devices
```

- ドライバのインストール
```ssh
$ sudo ubuntu-drivers autoinstall
```

- 参考
  - https://blog.hysakhr.com/2019/11/12/%E3%82%A8%E3%83%A9%E3%83%BC%E3%83%A1%E3%83%83%E3%82%BB%E3%83%BC%E3%82%B8%E3%80%8Cnvidia-smi-has-failed-because-it-couldnt-communicate-with-the-nvidia-driver-make-sure-that-the-latest-nvidia-driver/

## versionを指定して更新する方法

```sh
$ sudo apt purge nvidia-*
$ sudo add-apt-repository ppa:prahics-drivers
$ sudo add-apt-repository ppa:graphics-drivers
$ sudo apt update
$ sudo apt upgrade
$ sudo apt install nvidia-driver-470
$ curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey |   sudo apt-key add -
$ curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list |   sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
$ sudo apt update
$ sudo apt install nvidia-container-runtime
$ sudo apt install nvidia-docker2
```

## nvidia-smiを常駐させる方法

```sh
$ watch nvidia-smi
```

## Failed to initialize NVML: Driver/library version mismatch対応

- 根本はドライバアップデートした方がよさそう。

- アップデートしたくない場合はとりあえずドライバのアンロードを以下に沿って試す。
  - https://qiita.com/ell/items/be3d3527b723f70f888d
