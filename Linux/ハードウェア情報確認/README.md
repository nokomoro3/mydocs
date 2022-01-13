# ハードウェア情報確認

- OS
```sh
$ lsb_release -a
```

- カーネル
```sh
$ uname -a
```

- CPU
  - threadsがコア数と思えばよい。
```sh
$ sudo lshw -class processor
```

- GPU
```sh
$ lspci | grep -i nvidia
```

- RAM
```sh
$ sudo dmidecode -t memory
```

- マザーボード
```sh
$ sudo dmidecode -t baseboard
```

- 参考
  - https://qiita.com/sabaku20XX/items/97db2c0bf7298e3a645c
  - https://qiita.com/DaisukeMiyamoto/items/98ef077ddf44b5727c29

## プロセスの実行メモリ確認方法

- topコマンドでPIDを特定する。

- 以下のコマンドをwatchで実行してpeakを取得する。
```sh
watch grep ^VmPeak /proc/$PID/status
```