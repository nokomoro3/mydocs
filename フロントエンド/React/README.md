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
      "src": resolve(__dirname, "src"),
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

### ゼロからModalを作る

- https://reffect.co.jp/react/react-modal

