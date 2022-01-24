# html

## SVG

- M(Move)やL(Line)がコマンドとなっている。
  - M ... "Move To" コマンド。指定した位置に移動する。始点などに使う。
  - L ... "Line To" コマンド。指定した位置に直線をひく。
  - Z ... "Close Path" コマンド。終点などに使い、始点までの線をひく。

- 単純な三角形はこのようになる。
```html
<svg width="100" height="100">
  <path d="M50 0 L0 100 L100 100 Z" style="fill:red"></path>
</svg>
```

- 参考
  - https://developer.mozilla.org/ja/docs/Web/SVG/Tutorial/Paths
  - https://www.kipure.com/article/93/

## 半角スペースを入れる

```html
&nbsp;
```