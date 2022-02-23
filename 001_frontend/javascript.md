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

## JavaScriptで指定したN回分ループする

- https://qiita.com/taneba/items/04c99e236bc87dab59e0

## BOMやBlobを理解してJavaScriptでCSVを出力する

- https://qiita.com/megadreams14/items/b4521308d5be65f0c544


## varを使う３つの理由

- https://dev.to/paritho/3-reasons-to-use-var-in-javascript-1hoe