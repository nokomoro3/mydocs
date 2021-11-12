# WSL

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