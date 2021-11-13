# VSCode

## 日本語フォント設定

- HackGen
  - 色々試したが、HackGenが英文字がつぶれなくてかなり良い。
    - https://github.com/yuru7/HackGen/releases

- 以下は検討したが、英文字がつぶれてカクカクしてしまう。(解像度のせいかもしれんが)
  - Myrica
    - Myrica / MyricaMを以下よりダウンロード。
      - https://myrica.estable.jp/

    - zip解凍し、ttcファイルをダブルクリックしていインストール。

  - Ricty Diminished
    - https://github.com/edihbrandon/RictyDiminished

- 設定反映
  - VSCodeのフォントを設定し、VSCodeを再起動。

## プラグイン

- 基本
  - Japanese Language Pack ... 日本語化。

- 視認性
  - Brancket Pair Colorizer2 ... カッコを色分けしてくれる。
  - indent-rainbow ... インデントを色分けしてくれる。
  - Rainbow CSV ... CSVの視認性向上。
  - Trailing Space ... 空白の可視化

- Git
  - Git Graph ... 変更履歴がツリーで見れる。
  - GitLens ... ブランチ同士の差分確認で主に使う。

- Python
  - Python ... Python開発の基本。
  - Pylance ... 構文チェックや候補を頭良く表示する。今はPythonプラグインを入れると勝手に入る気がする。
  - Python Docstring Generator ... Pythonのdocstringを自動生成できる。

- Markdown
  - Markdown PDF ... MarkdownをPDF変換する。
  - Markdown Preview Enhanced ... Markdownの表示が見やすくなる。
  - Draw.io ... 図を書ける。*.drawio.svgで保存すれば、markdownに埋め込める。
  - PlantUML ... UMLを記述できる。図として出力し、markdownに埋め込める。

- フォーマッター
  - Prettier ... 自動整形ツール。あまりメリットをまだ感じられていないが、嬉しいのだろうか。

- リモート系
  - Remote SSH ... VSCodeからサーバーにSSH接続できる。
  - Remote Containers
  - Remote WSL

- フロントエンド
  - Auto Close Tag ... htmlタグを自動で閉じる。
  - Auto Rename Tag ... htmlタグを自動でリネームできる。
  - JavaScript (ES6) code snippets ... JavaScript用
  - ES7 React/Redux/GraphQL/React-Native snippets ... React開発用
  - ESlint ... linter
  - Vetur ... Vue開発用

- クラウド
  - Azure Account / Azure App Service / Azure Resources
