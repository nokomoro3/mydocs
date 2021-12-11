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
