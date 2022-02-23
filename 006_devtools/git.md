# Git

## 初期設定

- 以下がお勧めの初期設定
```bash
# 必須設定
git config --global core.autocrlf false
git config --global core.ignorecase false
git config --global color.ui true
git config --global core.quotepath false
git config --global core.filemode false

# 必要に応じて設定
git config --global user.name 'Your Name'
git config --global user.email 'example@example.com'
git config --global credential.helper 'cache --timeout=86400'
```

- 有効にしたはずなのにならない場合は、localと競合している可能性がある。

- 少なくとも以下は競合していたので、実行が必要かも。
```bash
git config --local --unset core.ignorecase
git config --local --unset core.filemode
```

## 設定確認

```bash
git config --list --local
git config --list --global
```

- 設定値を編集する場合
```bash
git config --global <設定キー名> <設定値>
```

- 設定値を削除するには、--unsetを使う。
```bash
git config --global --unset <設定キー名>
```

## Gitのバージョンアップ

- Linuxの場合
```bash
sudo add-apt-repository ppa:git-core/ppa
sudo apt update
sudo apt upgrade
```

- Windows(Git for Windows)の場合
```bash
git update-git-for-windows
```

## Git for Windowsのターミナルをタスクバーから起動した際にhomeから始める方法

- ショートカットのリンク先に以下のようなオプションをつける。
```
"C:\Program Files\Git\git-bash.exe" --cd-to-home
```

## Windowsで複数のgitアカウントを使い分ける。

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

## Gitの内部構造

- たぶんもう怖くないGit Git内部の仕組み
  - https://qiita.com/marchin_1989/items/2ec01553e907f3a9e6bb