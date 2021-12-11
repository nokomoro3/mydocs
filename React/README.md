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


### vite.config.ts

- importを絶対パスにする。

```vite.config.ts
 import { defineConfig } from 'vite';
 import react from '@vitejs/plugin-react';

 // https://vitejs.dev/config/
 export default defineConfig({
   root: './src',
   plugins: [react()],
 });
```
