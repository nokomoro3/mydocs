# React

## settings

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


### index.htmlをsrc配下に移動する。

- vite.config.tsのrootを'./src'にする。

```vite.config.ts
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
  
// https://vitejs.dev/config/
export default defineConfig({
  root: './src',
  plugins: [react()],
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
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

// https://vitejs.dev/config/
export default defineConfig({
  root: './src',
  build: {
    // root (= ./src) から見た相対パスで指定
    outDir: '../public',
    emptyOutDir: true,
  },
  plugins: [react()],
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

### 参考
- https://zenn.dev/sprout2000/articles/98145cf2a807b1

