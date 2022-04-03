# curl

## curlオプションとMIME type

- 以下のように、dオプションにすると、"application/x-www-form-urlencoded"となる。
```sh
$ curl -X POST -d @test.csv -H 'x-api-key:{APIキー}' -v https://localhost
```

- 以下のように、fオプションにすると、"multipart/form-data"となる。
```sh
$ curl -X POST -f 'upload=@test.csv' -H 'x-api-key:{APIキー}' -v https://localhost
```

- 参考
  - curl の POST オプション -d と -F の違いから、改めて MIME type を学ぶ
    - https://qiita.com/att55/items/04e8080d1c441837ad42

## curlの様々なd系オプション

- 以下が結論だが、--data-binaryが最も余計なことをしないと覚えておけば良さそう。

|オプション|@でファイル送信|@ファイル内の改行を削除|URLエンコード|
|:---|:---|:---|:---|
|-d, --data, --data-ascii|○|○|×|
|--data-raw|×|-|×|
|--data-binary|○|×|×|
|--data-urlencode|○|×|○|

- 参考
  - curlのオプション--data, --data-binary, --data-raw, --data-urlencodeの違い
    - https://qiita.com/aosho235/items/d89bb027db0c5662d8c5

## JSON整形

- curlってオプションに -w '%{json}' って渡すとJSONで吐いてくれるの知らなかった！便利！
  - https://twitter.com/yusukebe/status/1502499611038007297