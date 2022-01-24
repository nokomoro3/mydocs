# npm + yarn

## インストール

### MacOSの場合

- homebrewを入れる。
  - https://brew.sh/index_ja.html
  - 以下でインストールを確認
  ```bash
  $ brew -v
  # => 1.1.0#    Homebrew/homebrew-core (git revision bb24; last commit 2016-11-15)
  ```

- nodebrewを入れる。
```bash
$ brew install nodebrew
# ...
$ nodebrew -v
# => 0.9.6# ...(いろいろ表示される)
```

- 安定版のnodeをインストールする。
```bash
$ nodebrew install-binary stable
```

- PATHを通す
```bash
$ echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.bash_profile # bachの場合
$ echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.zprofile # zshの場合
# *** ターミナル再起動 ***
```

- nodeのバージョン確認
```bash
$ node -v
```

- yarnも入れる
```bash
$ npm install --global yarn
# ...(いろいろ表示される)
$ yarn --version
```

### Windows の場合

- 以下からnodeのインストーラ(LTS版)を取得して、インストールする。
  - https://nodejs.org/ja/

- インストール確認
```powershell
> node --version
::: v14.17.1
```

- yarnも入れる。
```powershell
> npm install --global yarn
::: ...
> yarn --version
::: 1.22.10
```

- スクリプト実行権限の設定(PowerShellのみ)
  - この項目は、自己責任で実施するか、PowerShell以外を使ってください。
  - PowerShellでvueコマンドを使う場合は、実行権限の変更が必要。
  - 管理者モードでPowerShellを起動する。以下で実行権限を確認する。
  ```powershell
  PS C:\WINDOWS\system32> PowerShell Get-ExecutionPolicy
  Restricted
  ```

  - 実行権限を以下で変更する。
  ```powershell
  PS C:\WINDOWS\system32> PowerShell Set-ExecutionPolicy RemoteSigned
  PS C:\WINDOWS\system32> PowerShell Get-ExecutionPolicy
  RemoteSigned
  ```

### WSL/Ubuntuの場合

```bash
sudo apt install -y nodejs npm
sudo npm install -g n
sudo n lts
# --- terminal reboot --- #

sudo apt purge -y nodejs npm
sudo apt autoremove -y
node -v
# -> v14.17.0

sudo apt install -y curl
curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | sudo apt-key add -
echo "deb https://dl.yarnpkg.com/debian/ stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt update && sudo apt install yarn
yarn --version
# -> 1.22.5
```
