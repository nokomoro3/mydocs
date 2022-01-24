# ネットワーク

## 使用中のポート番号を調べる。

- 使用中のPIDが分かる。
```sh
$ sudo lsof -i :8080
```

- PIDから使用中のプロセス詳細を調べる。
```sh
$ sudo ps uxwwa | grep "15731"
```

## 新しめのUbuntuでnetplanを使ってネットワーク設定する。

- Ubuntuでnetplanを使って固定IPアドレスを設定する方法
  - https://www.orzs.tech/configure-static-ip-address-using-netplan/

- Ubuntu 20.04 LTSで固定IPアドレスの設定
  - https://qiita.com/zen3/items/757f96cbe522a9ad397d


## IPアドレスの末尾の /24 とは何か

- サブネットマスクのことらしい。
- https://penpen-dev.com/blog/sabunetto/#:~:text=%E7%B5%90%E8%AB%96%E3%81%8B%E3%82%89%E8%A8%80%E3%81%86%E3%81%A8%E3%80%81IP,%E3%82%B5%E3%83%96%E3%83%8D%E3%83%83%E3%83%88%E3%83%9E%E3%82%B9%E3%82%AF%E3%81%A8%E3%81%84%E3%81%86%E6%83%85%E5%A0%B1%E3%82%89%E3%81%97%E3%81%84%E3%80%82&text=%E8%A6%81%E3%81%99%E3%82%8B%E3%81%AB%E3%80%81%E3%82%B5%E3%83%96%E3%83%8D%E3%83%83%E3%83%88%E3%83%9E%E3%82%B9%E3%82%AF%E3%82%92%E4%BD%BF%E3%81%86,%E3%81%99%E3%82%8B%E3%81%93%E3%81%A8%E3%81%8C%E3%81%A7%E3%81%8D%E3%82%8B%E3%82%89%E3%81%97%E3%81%84%E3%80%82