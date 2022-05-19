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

## 自己証明書の設定

- 証明書の作成（作成は別のマシンでもかまわない）

```
openssl req -new -x509 -sha256 -newkey rsa:2048 -days 365 -nodes -out nginx.pem -keyout nginx.key
```

- 適当な場所に複製

```
mkdir -p /etc/nginx/ssl/
cp nginx.key /etc/nginx/ssl/nginx.key
cp nginx.pem /etc/nginx/ssl/nginx.pem
```

- confのどこかに以下のように記述

```sh
server {
    listen 80;
    return 301 https://$host$request_uri;
}


server {
    listen 443 ssl;
    server_name _;

    ssl_certificate /etc/nginx/ssl/nginx.pem;
    ssl_certificate_key /etc/nginx/ssl/nginx.key;
}
```

- 参考
  - https://qiita.com/inakadegaebal/items/29d21d1f5a904a6ba92d
