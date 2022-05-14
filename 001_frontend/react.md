# React

## settings

### VSCodeプラグイン

- React開発効率を3倍にするVS Code拡張機能&環境設定
  - https://qiita.com/newt0/items/b7810fb38c339ec5e4a7

### create-react-app

- Reactのテンプレートを作成する。
```
$ npx create-react-app {プロジェクト名} --template typescript
```

### tsconfig.json

- importを絶対パスにする。
```
{
  "compilerOptions": {
    ...,
    "baseUrl": "src"
  },
  ...
}
```

## settings(use Vite)

### template

- react-tsのテンプレートを使う。
```
$ yarn create vite
...
success Installed "create-vite@2.7.0" with binaries:
      - create-vite
      - cva
√ Project name: » {プロジェクト名}
√ Select a framework: » react
√ Select a variant: » react-ts
...
Done in 75.97s.
$ cd {プロジェクト名}
$ yarn
$ yarn dev
```

### yarn devでブラウザを起動する。

- vite.config.tsでserver.open=trueとする。

```vite.config.ts
export default defineConfig({
  server: {
    open: true,
  },
})
```

### index.htmlをsrc配下に移動する。

- vite.config.tsのrootを'./src'にする。

```vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
  
// https://vitejs.dev/config/
export default defineConfig({
  root: './src',
});
```

- index.html内のfavicon.svgとmain.tsxのパスからsrcを削除する。
```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Vite App</title>
  </head>
  <body>
    <div id="root"></div>
    <script type="module" src="/main.tsx"></script>
  </body>
</html>
```

- mvする。
```
$ mv ./index.html ./src/
```

### ビルド先をpublicにする。

- vite.config.tsのbuild.outDirをroot(./src)からみたpath(../public)にする。
- vite.config.tsのbuild.emptyOutDirをtrueにすることで、ビルド前にクリアできる。

```vite.config.ts
export default defineConfig({
  root: './src',
  build: {
    // root (= ./src) から見た相対パスで指定
    outDir: '../public',
    emptyOutDir: true,
  },
});
```

- ビルド
```
$ yarn build
```

- 確認(preview)
```
$ yarn preview
```

### 絶対パスでimportできるようにする。

- vite.config.tsのresolve.aliasを設定する。
- pathがない場合は、`yarn add @types/node -D`で追加する。

```vite.config.ts
import * as path from "path"

export default defineConfig({
  resolve: {
    alias: {
      "src": path.resolve(__dirname, "src"),
    },
  },
})
```

- 次にtsconfig.jsonのcompilerOptions.baseUrlを設定する。

```tsconfig.json
{
  "compilerOptions": {
    "baseUrl":"./",
  },
}
```

- エディタ(VSCode)を再起動する。

### 参考
- Vite で最速 React & TypeScript
  - https://zenn.dev/sprout2000/articles/98145cf2a807b1
- Vite+React+Typescriptでimportのパスを必要最低限にする
  - https://qiita.com/okoshi/items/614211ae0f52cd2174ab


## Hooks

### 参考
- https://sbfl.net/blog/2019/11/12/react-hooks-introduction/

## ライブラリ

### Grid Layout

- React-Grid-LayoutでサクッとDrag & Dropできるコンポーネントを作る
  - https://qiita.com/duplicate1984/items/321805cf99ef7a9e4924

### react-transition-group

- CSSのtransitionを助けるreactコンポーネント
  - https://qiita.com/takeshisakuma/items/67578529789939c900ff


### react-tween

- https://qiita.com/nabepon/items/c005a7d4491fd04b453e

### react-router

- v5の入門
  - https://qiita.com/h-yoshikawa44/items/aa78b6c7cd1ef43549bf

- v5をv6に置き換え
  - https://dev.classmethod.jp/articles/react-router-5to6/

- 自作方法
  - https://zenn.dev/stin/articles/how-to-develop-react-router

### SWR(stale-while-revalidate)

- データフェッチのための React Hooks ライブラリ。

- 参考
  - https://zenn.dev/uttk/articles/b3bcbedbc1fd00

### Next.js

- 公式チュートリアルでNext.jsに入門してみた
  - https://dev.classmethod.jp/articles/introduction-to-nextjs/

### Recoil(GlobalState管理)

- Next.js + TypeScript + Recoil + Herp社ESLint Config でReactチュートリアルを作る。
  - https://zenn.dev/purenium/articles/nextjs-recoil-tic-tac-toe

- 公式マニュアル
  - https://recoiljs.org/docs/api-reference/core/useRecoilStateLoadable

### react-query

- データフェッチのための React Hooks ライブラリ。
  - https://react-query.tanstack.com/guides/queries

- react-queryをやるならまずここを読んだ方がいい
  - https://react-query.tanstack.com/community/tkdodos-blog

### ゼロからModalを作る

- https://reffect.co.jp/react/react-modal


### React hooksを基礎から理解する

- https://qiita.com/seira/items/0e6a2d835f1afb50544d

### Radix UI

- https://www.radix-ui.com/

### SWRとReact Query比較

- https://scrapbox.io/fsubal/SWR_vs_React_Query

### styled-components (CSS in JS)

- 次の時代のCSS in JSはWeb Componentsを従える
  - https://zenn.dev/uhyo/articles/next-gen-css-in-js

- 経年劣化に耐える ReactComponent の書き方
  - https://qiita.com/Takepepe/items/41e3e7a2f612d7eb094a
  - これはかなり良かった。

- ブログに CSS in JS 環境下での スタイル分離リファクタリングを施してみた
  - https://blog.ojisan.io/s-c-refactor/

- styled-componentsをさくっとやる（tsx対応）
  - https://zenn.dev/nbr41to/articles/fcb47ec1a2a77a1da3db

- 歴史的な観点でまとめてある。
  - https://qiita.com/lightnet328/items/218eb1c4a347302cc340

### useEffectとuseReducerを使った非同期処理

- https://qiita.com/st2222/items/80f1c7cd711ed6522d23

### Promise入門

- https://www.tohoho-web.com/ex/promise.html

### React.FCとReact.VFCは使わなくていい説

- https://kray.jp/blog/dont-have-to-use-react-fc-and-react-vfc/

### focus trapとは何だ？

- https://ichi.pro/react-konpo-nento-no-torappu-fuxo-kasu-178865351197081

### useEffect内のsetStateを減らすテクニック

- https://zenn.dev/ypresto/articles/02f7adcb7c57b4

### プロジェクトのデザイン

- フロントエンドのデザインパターン
  - https://zenn.dev/morinokami/books/learning-patterns-1/viewer/provider-pattern

- Next.js + TypeScript + Recoil + Herp社ESLint Config でReactチュートリアルを作る。
  - https://zenn.dev/purenium/articles/nextjs-recoil-tic-tac-toe
  - Recoilは割と良かった。
  - しかしアプリによってはGlobalState要らない説もある。(react-queryをうまく使って)

- 「3種類」で管理するReactのState戦略
  - https://zenn.dev/yoshiko/articles/607ec0c9b0408d

### Suspenseとは

- https://zenn.dev/uhyo/books/react-concurrent-handson/viewer/what-is-suspense