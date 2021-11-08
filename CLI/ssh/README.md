# ssh

## リモート設定

- ~/.ssh/configを以下のように設定する。
```sh
# 書き方
# Host <任意の名前>
#   HostName <接続先サーバーホスト名、あるいはIPアドレス>
#   User <ユーザ名>
#   Port <ポート番号>
#   IdentityFile <秘密鍵ファイル(必要ならば)>

# 具体例
Host sv100
  HostName 192.168.100.100
  Port 22
  User snakamura
  IdentityFile ~/.ssh/ssh_keys/id_rsa_snakamura
```

- これで以下で192.168.100.100に接続できるようになる。
```sh
# $ ssh <任意の名前>
$ ssh sv100
```

## ポート転送設定

- 踏み台サーバーを経由した接続に、localhostのポートの転送設定をする。

- ~/.ssh/configを以下のように設定する。
```sh
# 書き方
# Host <任意の名前>
#   HostName <踏み台サーバーホスト名、あるいはIPアドレス>
#   User <ユーザ名>
#   Port <ポート番号>
#   IdentityFile <秘密鍵ファイル(必要ならば)>

#   LocalForward <ローカルのポート番号> <踏み台サーバから見える転送先アドレス>:<転送先ポート番号>

# 具体例
Host sv100
  HostName 192.168.100.100
  Port 22
  User snakamura
  IdentityFile ~/.ssh/ssh_keys/id_rsa_snakamura

  LocalForward 1022 192.168.200.101:22
```

- まず、転送設定を以下のオプションで実行し、バックグラウンド稼働させる。
  - f がバックグラウンド
  - N がリモートコマンドなし
  - さらに C を付与すると、圧縮転送となる。
```sh
$ ssh <任意の名前> -N -f
```

- これで以下で192.168.200.101に接続できるようになる。
```sh
# $ ssh <ユーザ名>@localhost -p <ローカルのポート番号>
$ ssh snakamura@localhost -p 1022
```

- 以上の例は、ssh接続の時のものだが、転送先ポート番号に22以外のものを割り当てても良い。
  - 以下が実例
    - Windows Remote Desktop(RDP) ... 3389
    - Jupyter ... 8888

- 転送を切りたい場合は、psでプロセスを探して、
```sh
$ ps
      PID    PPID    PGID     WINPID   TTY         UID    STIME COMMAND
      730     683     730       1892  pty0      197609 11:00:13 /usr/bin/ps
      665       1     665      19764  ?         197609 10:56:24 /usr/bin/ssh
      683     682     683      22300  pty0      197609 10:57:01 /usr/bin/bash
      682       1     682      16468  ?         197609 10:57:01 /usr/bin/mintty
```

- sshというやつを落とせばよい。
```sh
$ kill -9 665
```

- WindowsでPC起動と同時にやっておきたい場合は、以下のようなvbsスクリプトをタスクスケジューラに設定する。
  - objWShell.runは、複数書いてもOK
```vbs
Set objWShell = CreateObject("Wscript.Shell") 
objWShell.run "ssh <任意の名前> -N -f", vbHide
objWShell.run "ssh <任意の名前> -N -f", vbHide
```

## 参考

- SSHによるポートフォワーディング
  - https://blue-red.ddo.jp/~ao/wiki/wiki.cgi?page=SSH%A4%CB%A4%E8%A4%EB%A5%DD%A1%BC%A5%C8%A5%D5%A5%A9%A5%EF%A1%BC%A5%C7%A5%A3%A5%F3%A5%B0

- SSHポートフォワーディングを知った話
  - https://qiita.com/Ayaka14/items/449e2236af4b8c2beb81

- 【SSH】踏み台サーバーを経由した多段SSH接続のやり方（.ssh/configの利用）
  - https://serip39.hatenablog.com/entry/2020/12/18/235800
