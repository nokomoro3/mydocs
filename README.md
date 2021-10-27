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