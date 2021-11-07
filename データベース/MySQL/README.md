# MySQL

## utf8は本当のutf8ではない。

- utf8mb4を使う必要がある。設定としては、utf8mb4_general_ciでOK。

- これをしていないと、「Incorrect string value: ...」というエラーが出る場合がある。

- 参考
  - https://mita2db.hateblo.jp/entry/2020/12/07/000000
  - https://qiita.com/houtarou/items/228086bbc7d1d10abdb9
