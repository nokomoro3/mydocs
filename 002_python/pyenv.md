# pyenv

## 概要

- Python自身のバージョンを切り替えるためのもの。

## インストール(Windows)

- pipが必要なので、まずはPythonをインストールする。
  - https://www.python.org/

- pipでpyenv-winをインストールする。
  - /c/User/{ユーザ―名}\.pyenv\pyenv-win
```powershell
> pip install pyenv-win --target $HOME\.pyenv
```

- 環境変数を追加する。(管理者としてPowerShellを起動)
```powershell
> [System.Environment]::SetEnvironmentVariable('PYENV',$env:USERPROFILE + "\.pyenv\pyenv-win\","User")
> [System.Environment]::SetEnvironmentVariable('PATH', $HOME + "\.pyenv\pyenv-win\bin;" + $HOME + "\.pyenv\pyenv-win\shims;" + $env:Path,"Machine")
```

## インストール(Mac)

- インストール
```shell
$ brew install pyenv
$ brew install pyenv-virtualenv
```

- 環境変数追加
```shell
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bash_profile
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bash_profile
```

## インストール(WSL)

- pyenvの取得
```shell
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
cd ~/.pyenv && src/configure && make -C src # optional
```

- 以下を入力
```shell
# the sed invocation inserts the lines at the start of the file
# after any initial comment lines
sed -Ei -e '/^([^#]|$)/ {a \
export PYENV_ROOT="$HOME/.pyenv"
a \
export PATH="$PYENV_ROOT/bin:$PATH"
a \
' -e ':a' -e '$!{n;ba};}' ~/.profile
echo 'eval "$(pyenv init --path)"' >>~/.profile

echo 'eval "$(pyenv init -)"' >> ~/.bashrc
```
- ここで一旦terminalを再起動。

- 依存パッケージをインストール。
```shell
sudo apt install -y pip libbz2-dev libssl-dev libreadline-dev libsqlite3-dev libffi-dev
```

## 使用方法

- インストール可能なバージョン一覧の確認
```shell
$ pyenv install --list
```
- インストール
```shell
$ pyenv install 3.8.10
```

- 現在のバージョン確認
```shell
$ pyenv version
```

- インストール済みのバージョン一覧確認
```shell
$ pyenv versions
```

- 全体で利用するバージョンの設定
```shell
$ pyenv global 3.8.10
```

- 特定のディレクトリ配下のみ利用するバージョンの設定
```shell
$ cd PROJECT_DIR
$ pyenv local 3.9.1
$ pyenv version
Python 3.9.1
```

## 参考
- Windows 10 で Python のインストールから Poetry と pyenv の利用
  - https://qiita.com/kerobot/items/3f4064d5174676080585
- pyenv 利用のまとめ
  - https://qiita.com/m3y/items/45c7be319e401b24fca8
