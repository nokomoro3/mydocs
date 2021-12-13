# mydocs

- CLI
  - [curl](./CLI/curl/README.md)
  - [ssh ](./CLI/ssh/README.md)

- Linux
  - [NVIDIAドライバ周り            ](./Linux/NVIDIAドライバ周り/README.md)
  - [USB_LANアダプター設定         ](./Linux/USB_LANアダプター設定/README.md)
  - [ディスプレイ周りの確認コマンド](./Linux/ディスプレイ周りの確認コマンド/README.md)
  - [ハードウェア情報確認          ](./Linux/ハードウェア情報確認/README.md)

- ML
  - Kaggle
    - [Kaggleチュートリアル](./ML/Kaggle/Kaggle.01tutorial.md)
    - [Kaggleコンペ一覧    ](./ML/Kaggle/Kaggle.02compe.md)

- Python
  - [syntax  ](./Python/syntax/README.md)
  - [assert  ](./Python/assert/README.md)
  - [base64  ](./Python/base64/README.md)
  - [datetime](./Python/datetime/README.md)
  - [except  ](./Python/except/README.md)
  - [pandas  ](./Python/pandas/README.md)
  - [poetry  ](./Python/poetry/README.md)
  - [pyenv   ](./Python/pyenv/README.md)
  - [pytest  ](./Python/pytest/README.md)
  - [requests](./Python/requests/README.md)

- Webフレームワーク
  - [FastAPI](./Webフレームワーク/FastAPI/README.md)
  - [Nginx  ](./Webフレームワーク/Nginx/README.md)

- クラウド
  - [Azure](./クラウド/Azure/README.md)

- データベース
  - [MySQL](./データベース/MySQL/README.md)
  - [データベース設計](./データベース/データベース設計/README)

- ライブラリ
  - [画像処理](./ライブラリ/画像処理/README.md)

- 開発ツール
  - [Anaconda](./開発ツール/Anaconda/README.md)
  - Docker
    - [Dockerチュートリアル](./開発ツール/Docker/Docker.01tutorial.md)
    - [Docker on WSL2      ](./開発ツール/Docker/Docker.02WSL.md)
    - [Dockerノウハウ      ](./開発ツール/Docker/Docker.99knowhow.md)
  - [Git     ](./開発ツール/Git/README.md)
  - [GitHub  ](./開発ツール/GitHub/README.md)
  - [Hasura  ](./開発ツール/Hasura/README.md)
  - [npm_yarn](./開発ツール/npm_yarn/README.md)
  - [React   ](./開発ツール/React/README.md)
  - [VSCode  ](./開発ツール/VSCode/README.md)
  - [Vue     ](./開発ツール/Vue/README.md)
  - [WSL     ](./開発ツール/WSL/README.md)


## sandbox

- セキュリティ
  - ssl
    - 暗号化の設定を調べる。
    ```shell
    $ openssl s_client -connect localhost:443 -showcerts
    ```

- pythonで再帰処理

```python
def exec_lower_recurisive(text_input, text_output_recursive=[]):
    for element in text_input:
        if isinstance(element, list):
            text_output_sub = []
            exec_lower_recurisive(element, text_output_sub)
            text_output_recursive.append(text_output_sub)
        else:
            output = element.lower()
            text_output_recursive.append(output)

text_input = ["AAA", "BBB", "CCC", ["DDD", "EEE"] ]
text_output_recursive = []
exec_lower_recurisive(text_input, text_output_recursive)

print(text_output_recursive)
# OUT: ['aaa', 'bbb', 'ccc', ['ddd', 'eee']]
```

- pythonのsetの使い方
  - https://uxmilk.jp/14834

- jupyter
  - markdownの表を全体左に寄せる。
    - 冒頭に以下のhtmlを書く。
    ```
    %%html
    <style>
    table {float:left}
    </style>
    ```
  - コンソールコマンドに変数を渡す方法
    - https://qiita.com/piruty/items/9c4f87cc2240e1028b31

- MySQL
  - 外部キー制約
    - https://qiita.com/petitviolet/items/e03c67794c4e335b6706
    - https://qiita.com/suin/items/21fe6c5a78c1505b19cb
    - https://hacknote.jp/archives/19470/

- pythonでhtmlの特殊文字をエスケープする
  - https://docs.python.org/ja/3/library/html.html

- sqlalchemyのrelationshipの設定
  - https://qiita.com/1234224576/items/ba66838b32b99cce51d2
  - https://poyo.hatenablog.jp/entry/2017/01/08/212227

- javascript
  - JavaScriptで指定したN回分ループする
    - https://qiita.com/taneba/items/04c99e236bc87dab59e0
  - BOMやBlobを理解してJavaScriptでCSVを出力する
    - https://qiita.com/megadreams14/items/b4521308d5be65f0c544

- msgconvert
  - msgをemlに変換するツール。
  ```sh
  sudo apt install libemail-outlook-message-perl libemail-sender-perl
  ```
  
- MeCabへの単語追加
  - https://taku910.github.io/mecab/dic.html
  - https://www.koi.mashykom.com/nlp.html
  - https://qiita.com/hiro0217/items/cfcf801023c0b5e8b1c6
  - 
