# poetry

## poetryインストール

- powershellの場合
```ps
> (Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```
- 以下にインストールされる。
  - /c/Users/{ユーザ名}/.poetry/bin

- linux/WSL/macOSの場合
```sh
$ curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

- 以下にインストールされる。
  - $HOME/.poetry/bin

## poetryの設定

- 設定の確認。
```sh
$ poetry config --list # global
$ poetry config --local --list # local
```

- 値を設定したいときは以下のようにする。
```sh
$ poetry config {設定名} true
```

- 値をデフォルトにしたい場合は、--unsetする。
```sh
$ poetry config {設定名} --unset
```

- デフォルトの設定は以下の通り。
```
cache-dir = "C:\\Users\\{ユーザ名}\\AppData\\Local\\pypoetry\\Cache"
experimental.new-installer = true
installer.parallel = true
virtualenvs.create = true
virtualenvs.in-project = null
virtualenvs.path = "{cache-dir}\\virtualenvs"  # C:\Users\{ユーザ名}\AppData\Local\pypoetry\Cache\virtualenvs
```

- `virtualenvs.in-project`は、仮想環境のインストール場所を設定する。
- プロジェクトと同じ場所に`.venv`として配置したいのであれば、`true`にする必要がある。
- デフォルトの`null`の場合、`C:\Users\{ユーザ名}\AppData\Local\pypoetry\Cache\virtualenvs`に配置される。

## poetryの基本的な使い方

- 仮想環境一覧の確認
```sh
$ poetry env list
```

- 仮想環境の情報取得
```sh
$ poetry env info
```

- 仮想環境へのログイン
```sh
$ poetry shell
```

- 仮想環境でコマンド実行
```sh
$ poetry run {your commands}
```

- poetryでの管理を開始する。
```sh
$ poetry init
```

- 既にあるtomlファイルを元に環境を構築する。
```sh
$ poetry install
```

- パッケージを追加する。
```sh
$ poetry add {Package Name}
```

- パッケージを削除する。
```sh
$ poetry remove
```

## poetry自体のアップデート

- こういうのもある。
```
> poetry self update
```

- なんかうまくいかないときは、普通にインストール時のコマンドを試す価値あり。
```
> (Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```

## poetry完全削除

- インストール場所の削除
  - Windowsの場合: C:\Users\{ユーザ名}\.poetry\bin
  - Linux/WSL/macOSの場合: $HOME/.poetry/bin

- キャッシュの削除
  - Windowsの場合: 
    - C:\Users\{ユーザ名}\AppData\Local\pypoetry
    - C:\Users\{ユーザ名}\AppData\Roaming\pypoetry
    - C:\Users\{ユーザ名}\AppData\Roaming\Python
  - Linux/WSL/macOSの場合: ??

## トラブルシューティング

### キャッシュからのパッケージ読み込みエラー

- 以下ののようなエラー。
```
C:\Users\{ユーザ名}\AppData\Local\pypoetry\Cache\artifacts\ ...(略)... does not exist
```

- キャッシュが読み込めないエラーがあるっぽい。
  - https://blog.panicblanket.com/archives/6329

- githubのissueはcloseしている。
  - https://github.com/python-poetry/poetry/issues/4163

- 結論としてよくわからないパッケージ見つからないエラーがでた場合、以下のキャッシュを消すことは試す価値がある。
```
C:\Users\{ユーザ名}\AppData\Local\pypoetry\Cache\artifacts
```