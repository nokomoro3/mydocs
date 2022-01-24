# Vue

## vue/cliインストール

```bash
> npm install -g @vue/cli
# $ sudo npm install -g @vue/cli # linux, WSLの場合
```

## vue/cliでプロジェクト作成

```
$ vue create my-project
```

- 設定は以下の通り。

```bash
Vue CLI v4.5.13? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, TS, Router, Vuex, CSS Pre-processors, Linter, Unit, E2E
? Choose a version of Vue.js that you want to start the project with 3.x
? Use class-style component syntax? No
? Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? Yes
? Use history mode for router? (Requires proper server setup for index fallback in production) Yes
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Sass/SCSS (with dart-sass)
? Pick a linter / formatter config: Standard
? Pick additional lint features: Lint on save, Lint and fix on commit
? Pick a unit testing solution: Jes
t? Pick an E2E testing solution: Cypress
? Where do you prefer placing config for Babel, ESLint, etc.?: In dedicated config files
? Pick the package manager to use when installing dependencies: (Use arrow keys): Use Yarn
```

- 以下一つずつ説明。
- まず、マニュアルにする。

```
? Please pick a preset: Manually select features
```

- 次で設定する項目を選ぶ。PWA以外は有効とした（PWAはスマホ対応なので要らないと考えた）。

```
? Check the features needed for your project: Choose Vue version, Babel, TS, Router, Vuex, CSS Pre-processors, Linter, Unit, E2E
```

- vueのversionは新しい方を選択。

```
? Choose a version of Vue.js that you want to start the project with 3.x # vueは3.xを使用
```

- Noとする。vue3では、class-styleではなくcomposition APIという方がスタンダードらしい。
- React Hooksのうな型推論の恩恵を受けれるというのが自分的には好印象だったので。
    - https://qiita.com/jesus_isao/items/0b305c7d90c45ad66c1b

```
? Use class-style component syntax? No
```

- ここはデフォルトのYes。

```
? Use Babel alongside TypeScript (required for modern mode, auto-detected polyfills, transpiling JSX)? Yes
```

- サーバー側で対応が必要だが、SPAで操作履歴を見るためには、historyがあった方が都合よさそうなのでYes。

```
? Use history mode for router? (Requires proper server setup for index fallback in production) Yes
```

- 生CSSは辛そうなので、新しいdart-SCSSを使ってみる。

```
? Pick a CSS pre-processor (PostCSS, Autoprefixer and CSS Modules are supported by default): Sass/SCSS (with dart-sass)
```

- Linterの設定。airbnbは結構厳しいみたいなので、Standardにした。
- Prettierは後でインストールする。
- 忘れないよう保存時に強制適用にする。

```
? Pick a linter / formatter config: Standard? Pick additional lint features: Lint on save, Lint and fix on commit
```

- テストフレームワークは標準になってきているJestを設定

```
? Pick a unit testing solution: Jest
```

- E2EテストはCypressが良いらしい。

```bash
? Pick an E2E testing solution: Cypress
```

- 各設定ファイルは別ファイルに合った方がいいかな。

```bash
? Where do you prefer placing config for Babel, ESLint, etc.?: In dedicated config files
```

- yarn, npmどちらを使うか聞かれたらyarnを選択。

```bash
? Pick the package manager to use when installing dependencies: (Use arrow keys): Use Yarn
```

## サーバー起動

```
$ cd my-project
$ yarn serve
```