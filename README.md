# mydocs

## 開発ツール

### Git

#### Windowsで複数のgitアカウントを使い分ける。

- Git for Windowsは必要。credential.namespaceを使えばできる。

- オプション付きでcloneする。
```
git -c credential.namespace=xxx clone https://github.com/[name]/[repository].git
```

- repositoryに移動し、gitのlocal configでnamespaceを設定する。
```
cd [repository]
git config --local credential.namespace xxx
```

- 参考
  - https://qiita.com/shiena/items/fc7783a82d59be5ff259


### Docker

#### 特定のRUN以降のキャッシュを無効にする。

- ARGを適当な場所に入れる。

```yml
ARG CACHEBUST=1
```

- build時に引数を設定する。
```
$ docker build -t tagname --build-arg CACHEBUST=$(date +%s) .
```

- 参考
  - https://www.chazine.com/archives/4038

#### コンテナを起動したままにする方法。

- 以下で起動状態を維持できる。
```sh
$ docker run -itd {イメージ名}:{タグ} /bin/sh
$ docker exec -it {イメージ名}:{タグ} /bin/sh # その後、これでログイン
```

- docker-composeで、tty:trueとしてもできる。
```yml
version: '2'
services:
  sample:
    image: {イメージ名}:{タグ名}
    container_name: sample_container
    tty: true
```

- 参考
  - Dockerのコンテナを起動したままにする
    - https://qiita.com/reflet/items/dd65f9ffef2a2fcfcf30

## クラウド

### Azure

#### Azure Blob StorageとAzure Filesの違い

- Filesの方が普通のファイルシステム、BlobはHTTPでのアクセスとなる。
  - https://docs.microsoft.com/ja-jp/azure/storage/files/storage-files-faq

- 普通にローカルマシンにもマウントできるようだ。


## Database

### Database設計

#### indexとは何か

- indexのメリット・デメリット
  - メリット：データの読み込み・取得が早くなる。
  - デメリット：書き込みの速度が倍かかる。

- indexがあると、検索が速くなる。
- その分、書き込むときは遅くなる。

- 参考
  - https://qiita.com/seiya1121/items/fb074d727c6f40a55f22


### MySQL

#### utf8は本当のutf8ではない。

- utf8mb4を使う必要がある。設定としては、utf8mb4_general_ciでOK。

- これをしていないと、「Incorrect string value: ...」というエラーが出る場合がある。

- 参考
  - https://mita2db.hateblo.jp/entry/2020/12/07/000000
  - https://qiita.com/houtarou/items/228086bbc7d1d10abdb9


## Webフレームワーク

### FastAPI

#### 全般

- https://qiita.com/qiuyin/items/ce6a3c31d13a5403ad67

## ライブラリ

### 画像処理

#### PythonでOCR(Tesseract + PyOCR)

- https://rightcode.co.jp/blog/information-technology/python-tesseract-image-processing-ocr

#### pdftoppm(PDFを画像に変換)

- https://atmarkit.itmedia.co.jp/ait/articles/1903/08/news039.html

## Python

### requests

#### json形式でバイナリファイルを送信する方法

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

### multipart/form-dataでバイナリファイルを送信する方法

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

## コマンド

### curl

#### curlオプションとMIME type

- 以下のように、dオプションにすると、"application/x-www-form-urlencoded"となる。
```sh
$ curl -X POST -d @test.csv -H 'x-api-key:{APIキー}' -v https://localhost
```

- 以下のように、fオプションにすると、"multipart/form-data"となる。
```sh
$ curl -X POST -f 'upload=@test.csv' -H 'x-api-key:{APIキー}' -v https://localhost
```

- 参考
  - curl の POST オプション -d と -F の違いから、改めて MIME type を学ぶ
    - https://qiita.com/att55/items/04e8080d1c441837ad42

#### curlの様々なd系オプション

- 以下が結論だが、--data-binaryが最も余計なことをしないと覚えておけば良さそう。

|オプション|@でファイル送信|@ファイル内の改行を削除|URLエンコード|
|:---|:---|:---|:---|
|-d, --data, --data-ascii|○|○|×|
|--data-raw|×|-|×|
|--data-binary|○|×|×|
|--data-urlencode|○|×|○|


- 参考
  - curlのオプション--data, --data-binary, --data-raw, --data-urlencodeの違い
    - https://qiita.com/aosho235/items/d89bb027db0c5662d8c5