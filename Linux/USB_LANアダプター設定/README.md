# USB_LANアダプター設定

- lsusbで接続を確認。Linuxが認識したUSB-NIC情報が確認できる。
```sh
$ lsusb
Bus 002 Device 006: ID 0b95:1790 ASIX Electronics Corp. AX88179 Gigabit Ethernet
Bus 002 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 004 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 003 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 001 Device 002: ID 8087:0024 Intel Corp. Integrated Rate Matching Hub
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

- nmcliでインターフェース名もわかる。
```sh
$ nmcli d
DEVICE           TYPE      STATE        CONNECTION
enp0s25          ethernet  connected    有線接続 1
enxbc5c4ce12dd4  ethernet  unavailable  --
wlp3s0           wifi      unavailable  --
lo               loopback  unmanaged    --
```

- ifconfigで割り当てがわかる。
```sh
$ ifconfig
enp0s25: flags=4163  mtu 1500
        inet 172.16.1.2  netmask 255.255.255.0  broadcast 172.16.1.255
        inet6 fe80::5750:af67:9978:c952  prefixlen 64  scopeid 0x20
        ether 04:20:9a:41:1f:35  txqueuelen 1000  (Ethernet)
        RX packets 73036218  bytes 14125558901 (14.1 GB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 83834353  bytes 29466284783 (29.4 GB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
        device interrupt 20  memory 0xf7c00000-f7c20000

enxbc5c4ce12dd4: flags=4099  mtu 1500
        ether bc:5c:4c:e1:2d:d4  txqueuelen 1000  (Ethernet)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 2343183  bytes 468714804 (468.7 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 2343183  bytes 468714804 (468.7 MB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```

- lshwで詳しいNIC情報まで調べられる。
```sh
$ lshw -class network
```

- ちなみに、LANケーブルを実際に刺さなくてもここまで確認できる。

- 外部インターネット接続はpingやcurlで確認する。
```sh
$ ping www.google.com
$ curl www.google.com
```

- 名前解決できない場合は、DNS設定を見直す。

- 名前解決しているのに、到達できない「destination host unreachable」場合は、使っているネットワーク名を絞るため、通常の有線側を落とす。
```sh
$ sudo ifdown enp0s25
```

- 参考
  - https://www.gadgets-today.net/?p=5633
