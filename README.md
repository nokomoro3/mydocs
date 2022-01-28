# mydocs

- 001_frontend
  - [CSS       ](./001_frontend/css.md)
  - [HTML      ](./001_frontend/html.md)
  - [JavaScript](./001_frontend/javascript.md)
  - [React     ](./001_frontend/react.md)
  - [TypeScript](./001_frontend/typescript.md)
  - [Vue       ](./001_frontend/vue.md)

- 002_python
  - [assert    ](./002_python/assert.md)
  - [base64    ](./002_python/base64.md)
  - [celery    ](./002_python/celery.md)
  - [datetime  ](./002_python/datetime.md)
  - [except    ](./002_python/except.md)
  - [fastapi   ](./002_python/fastapi.md)
  - [logging   ](./002_python/logging.md)
  - [NumPy     ](./002_python/numpy.md)
  - [pandas    ](./002_python/pandas.md)
  - [poetry    ](./002_python/poetry.md)
  - [Pydantic  ](./002_python/pydantic.md)
  - [pyenv     ](./002_python/pyenv.md)
  - [pytest    ](./002_python/pytest.md)
  - [requests  ](./002_python/requests.md)
  - [seaborn   ](./002_python/seaborn.md)
  - [sqlalchemy](./002_python/sqlalchemy.md)
  - [syntax    ](./002_python/syntax.md)
  
- 003_linux
  - [curl                 ](./003_linux/curl.md)
  - [ディスプレイ設定     ](./003_linux/display_config.md)
  - [ハードウェア情報確認 ](./003_linux/hardware_specs.md)
  - [ネットワーク設定     ](./003_linux/network_config.md)
  - [nginx                ](./003_linux/nginx.md)
  - [NVIDIAドライバ設定   ](./003_linux/nvidia_driver.md)
  - [ssh                  ](./003_linux/ssh.md)
  - [USB_LANアダプター設定](./003_linux/usb_lan_adapter.md)

- 004_cloud
  - [Azure](./004_cloud/azure.md)

- 005_db
  - [データベース設計](./005_db/db_design.md)
  - [MySQL           ](./005_db/mysql.md)

- [006_devtools](006_devtools/README.md)
  - [Anaconda](./006_devtools/Anaconda.md)
  - Docker
    - [Dockerチュートリアル](./006_devtools/docker_001_tutorial.md)
    - [Docker on WSL2      ](./006_devtools/docker_002_wsl.md)
    - [Dockerノウハウ      ](./006_devtools/docker_003_knowhow.md)
  - [Git     ](./006_devtools/git.md)
  - [GitHub  ](./006_devtools/github.md)
  - [npm_yarn](./006_devtools/npm_yarn.md)
  - [PlantUML](./006_devtools/plantuml.md)
  - [VSCode  ](./006_devtools/vscode.md)
  - [WSL     ](./006_devtools/wsl.md)

- 007_service
  - [Hasura  ](./007_service/hasura.md)

- 008_test
  - [E2Eツール](./008_test/e2e.md)

- 100_ML
  - Kaggle
    - [Kaggleチュートリアル](./100_ML/kaggle_001_tutorial.md)
    - [Kaggleコンペ一覧    ](./100_ML/kaggle_002_competition.md)
  - 設計
    - [機械学習システム デザインパターン(メルカリ)](./100_ML/ml_system_design.md)

- 101_image
  - [画像処理](./101_image/README.md)

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

- なぜエンジニアが作る画面はダサいのか…?「理由」と「対策」を徹底解説
  - https://qiita.com/mskmiki/items/544149987475719e417b

- React hooksを基礎から理解する
  - https://qiita.com/seira/items/0e6a2d835f1afb50544d

- 個人開発
  - 30個以上の個人開発を失敗。そこから自分のサービスで生きていけるようになるまでの話。｜入江 慎吾 🚀 MENTA代表｜note
    - https://note.com/iritec/n/n17c741c5f02d