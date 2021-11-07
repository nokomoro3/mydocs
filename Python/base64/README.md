# base64

## base64概要

- 64種類の文字を用いて、データをエンコードする方式
- 64種類は以下の数字。
  - A-Zの26文字
  - a-zの26文字
  - 0-9の10文字
  - +と/
- それとは別に、padding用の=も使われる。
- データサイズは1.37倍に増える。
- 利用例は以下のようなケースである。
  - 電子メールの添付ファイル
  - Basic認証

## Python実装方法

- base64というライブラリを使う。
- 入出力は双方ともbytes型となる。
- そのため、文字列として扱いたい場合は、さらにdecode('utf-8')が必要
```python
import base64
file_data = open("test.csv", "rb").read()
b64encoded = base64.b64encode(file_data) # bytes
b64encoded_str = base64.b64encode(file_data).decode('utf-8') # str
```
