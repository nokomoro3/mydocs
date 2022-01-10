# JavaScript

## axios

- 【Ajax】axiosを使って簡単にHTTP通信
  - https://www.willstyle.co.jp/blog/2751/

- catchしたerrorがAxiosErrorか判定する
  - https://zenn.dev/wintyo/articles/b417e310efc67e

## openapi generator

- openapitoolsのインストール
```sh
yarn add @openapitools/openapi-generator-cli -D
```

- 実行
```sh
npx openapi-generator-cli generate -g typescript-axios -i http://localhost:17100/api/v1/openapi.json -o ./src/api
```