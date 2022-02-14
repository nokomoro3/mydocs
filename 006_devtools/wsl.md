# WSL

## install

- https://docs.microsoft.com/ja-jp/windows/wsl/install-manual#step-3---enable-virtual-machine-feature

## aptが遅い場合

```
$ sudo sed -i.org -e "s/\/\/archive\.ubuntu\.com/\/\/jp\.archive\.ubuntu\.com/g" /etc/apt/sources.list
```

## 良く使うコマンド

- 今のWSL一覧を確認
```powershell
> wsl -l -v
> wsl --list --verbose
```

- 稼働中のWSLのバージョンを2に変更
```powershell
> wsl --set-version Ubuntu-20.04 2
```

- デフォルトバージョンをWSL2に変更
```powershell
> wsl --set-default-version 2
```

- デフォルトのWSLをUbuntu-20.04に変更
```powershell
> wsl --set-default Ubuntu-20.04
```

- WSLのシャットダウン
```powershell
> wsl --shutdown
```
