## HOG + SVM @2005

- 原論文
  - http://lear.inrialpes.fr/people/triggs/pubs/Dalal-cvpr05.pdf

- 概要
  - HOG特徴量を使ったSVM識別を矩形窓をスライドさせながら認識する。
  - 計算コストが低いため、リアルタイムで動作させることができる。
  - HOGはHistogram of Oriented Gradients。

- 手順
  - 固定サイズ窓サイズで抽出し、それを 8x8 pixelのcellに分割。
  - 各セルのHOG特徴量を計算する。
    - HOG特徴量は、cell内のpixelについて勾配強度を求め、それを8方向のヒストグラムで表したもの。
    - 詳細は下記参照。
      - https://qiita.com/chama0623/items/76568e7f16bc5e46d9c5#hog%E7%89%B9%E5%BE%B4%E9%87%8F
      - https://japimage.blogspot.com/2016/07/hog.html
  - HOG特徴量からSVMでclassificationを行う。(他クラス分類の非線形SVM)
  - 重複領域は、NMSで削除する。