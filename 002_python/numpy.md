# NumPy

- 歴史ある数値計算ライブラリ。
- 以外とこれだけでできることも多い。

## np.cumsum(累積和)

```py
import numpy as np
np.cumsum([1,2,3,4,5,6,7,8,9,10])
```

## np.flip(反転)
W.I.P.

## ブロードキャスト

- numpyのブロードキャストについてまとめ
  - AとBの演算したい場合に、shapeの長さが同じだと、1の部分の次元の要素を複製して演算できる。
    - shape=(4,3,2)とshape=(1,3,2)の演算はOK（左側に1があってもOK）
    - shape=(4,3,2)とshape=(4,3,1)の演算はOK（左側に1があってもOK）
    - shape=(5,4,3,2)とshape=(5,4,1,1)の演算はOK（1が複数あってもOK）
    - shape=(5,4,3,2)とshape=(1,4,1,1)の演算はOK（1が飛んでてもOK）
    - shape=(5,1,3,2)とshape=(5,4,1,2)の演算はOK（お互い違う場合でもOK）
  - また、shapeの長さが異なる場合でも、親側（左側）には1を自動追加することができる。
    - shape=(5,4,3,2)とshape=(4,3,2)の演算はOK（shape=(4,3,2)は勝手に(1,4,3,2)と解釈）
    - shape=(5,4,3,2)とshape=(3,2)の演算はOK（shape=(3,2)は勝手に(1,1,4,3,2)と解釈）
  - ちなみに、子側（右側）には1を自動追加できない。理由は両方に自動追加できると、複数の選択肢が発生するためと思われる。
    - shape=(5,4,3,2)とshape=(5,4)の演算はNG
    - やりたいときは、足りない側を明示的にreshapeしてあげる必要がある。

- 参考
  - https://snowtree-injune.com/2020/06/14/broadcast-z007/
