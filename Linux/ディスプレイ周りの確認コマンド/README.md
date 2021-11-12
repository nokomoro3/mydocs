# ディスプレイ周りの確認コマンド

- デバイスの確認
```sh
$ lspci | grep -i nvidia
```

- ドライバの確認
```sh
$ dpkg -l | grep nvidia
```

- X serverのログ確認
```sh
$ tail /var/log/Xorg.0.log
```

- X server の停止、開始
```sh
$ sudo stop lightdm
$ sudo start lightdm
```
