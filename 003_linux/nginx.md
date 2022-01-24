# Nginx

## BASIC認証設定

- 簡単に設定できる。

- htpasswdをインストール。
```sh
$ sudo apt install apache2-utils # Debian系
$ sudo yum install httpd-tools # RedHat系
```

- username/passwordを作成
```sh
sudo htpasswd -c -b /etc/nginx/.htpasswd {username} {password}
```

- nginx設定ファイルの編集
```conf
location / {
    auth_basic "Restricted";                   # 認証時に表示されるメッセージ
    auth_basic_user_file /etc/nginx/.htpasswd; # .htpasswdファイルのパス
}
```

- 参考
  - https://qiita.com/kotarella1110/items/be76b17cdbe61ff7b5ca
  - https://qiita.com/You_name_is_YU/items/e8db11eaa10067556e52
