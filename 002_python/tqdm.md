# tqdm

プログレスバー表示ライブラリ

## 基本

導入前。

```python
import time
for i in range(1000):
  time.sleep(1)
```

これにtqdmで囲ってあげるだけでOK。

```python
import time
from tqdm import tqdm
for i in tqdm(range(1000)):
  time.sleep(1)
```

`ascii=True`にしておくと表示が安定する。

```python
import time
from tqdm import tqdm
for i in tqdm(range(1000), ascii=True):
  time.sleep(1)
```

## generatorと一緒に使う

barがうまく表示されない場合は、generatorとなっています。

そういう場合は、`total`で全体の長さを指定してあげればOKです。

```python
import time
from tqdm import tqdm
for i in tqdm(range(1000), ascii=True, total=len(1000)):
  time.sleep(1)
```

## 確認用のカスタマイズした表示を入れる

`desc`で固定の名前を付けられます。

```python
import time
from tqdm import tqdm
for i in tqdm(range(1000), ascii=True, desc='loop1'):
  time.sleep(1)
```

動的に変更したい場合は、withでくくってあげる必要があります。

```python
import time
from tqdm import tqdm
with tqdm(range(1000), ascii=True) as pbar:
    for i in pbar:
        pbar.set_description("begin")
        time.sleep(1)
        pbar.set_description("end")
```

なお、`desc`の代わりに`postfix`を、`set_description`の代わりに`set_postfix`を使うことで、末尾側に表示を入れることが可能

## 参考

- グレートなtqdmの使い方
  - https://qiita.com/SeeLog/items/73c054a37722890b17a2
