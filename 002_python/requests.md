# requests

## json形式でバイナリファイルを送信する方法

- base64に一度エンコードし、utf-8でデコードし文字列にして送信する。

```python
import requests
import json
import base64

url = 'http://localhost'
headers = { "x-api-key": "APIキーなど" }

file_data = open("test.csv", "rb").read()
b64encoded = base64.b64encode(file_data).decode('utf-8')
json_data = {"file_data": b64encoded, "file_name": "test.csv"}

response = requests.post(url, data = json.dumps(json_data), headers = headers)
print(response.json())
```

- 受信側では、逆方向にutf-8でencodeし、base64でデコードする。
  - デコード結果はバイナリであるため、csvの場合は更にここからdecode('utf-8')をすれば文字情報になる。

```python
import base64
base64.b64decode(b64encoded.encode('utf-8'))
# base64.b64decode(b64encoded.encode('utf-8')).decode('utf-8') # さらにdecodeすれば文字列になる
```

# multipart/form-dataでバイナリファイルを送信する方法

- こちらの方法では、シンプルにバイナリのまま送信できる。

```python
import requests

url = 'http://localhost'
headers = { "x-api-key": "APIキーなど" }

file_data = open("test.csv", 'rb').read()
files = { 'uploadFile': ("test.csv", file_data, "text/csv") }

response = requests.post(url, headers=headers, files=files)
print(response.json())
```
