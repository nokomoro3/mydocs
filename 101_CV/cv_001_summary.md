
# CNNの歴史をひも解く

## そもそも画像認識のタスクって何がある？

- Classification
  - この画像は何？、この画像は何？というのを判断する問題。
  - 最も単純な問題ともいえる。

- Semantic segmentation
  - ピクセル単位でクラス分類を行うタスク
  - 概要はここが分かりやすい。
    - https://www.youtube.com/watch?v=Eu7EKQ--Rvk

- Object detection(物体検出)
  - 四角いbounding boxを同定。
  - そのboxのカテゴリは何？という2段階問題。
  - 概要はここが分かりやすい。
    - https://www.youtube.com/watch?v=5nmVHoA-A2E

- Instance Segmentation
  - pixcel単位にクラス分類した上で、同クラスで複数の物体は別物として認識する。
  - Semantic segmentation + Object detectionというイメージ。
  - こちらもObject Detection同様、bounding boxが存在。

- その他
  - 異常検知
  - 超解像

## Classification(Backbone)

[](./cv_history_002_classification.md)

## Object detection

[](./cv_history_003_object_detection.md)



## Segmentation

- 歴史が記載された図
  - https://twitter.com/ZFPhalanx/status/1266391941589024768?ref_src=twsrc%5Etfw%7Ctwcamp%5Etweetembed%7Ctwterm%5E1266391941589024768%7Ctwgr%5E%7Ctwcon%5Es1_&ref_url=https%3A%2F%2Fblog.negativemind.com%2Fportfolio%2Fdeep-learning-semantic-segmentation-algorithm%2F

- 【W.I.P.】U-Net @2015.05 (https://arxiv.org/pdf/1505.04597.pdf)
  - U-Net構造といわれる階層的なskip-connectionにより高解像データを失わない工夫をしたモデル。
- 【W.I.P.】Dilated Convolution @2015.11 (https://arxiv.org/pdf/1511.07122.pdf)
  - 間引きConvを重ねることでより広い領域からの情報を得ることを工夫したモデル。
- 【W.I.P.】SSD @2015.12 (https://arxiv.org/pdf/1512.02325.pdf)
- 【W.I.P.】Stacked Hourglass Networks @2016.03 (https://arxiv.org/pdf/1603.06937.pdf)
- 【W.I.P.】FPN: Feature Pyramid Networks @2016.12 (https://arxiv.org/pdf/1612.03144.pdf)
- 【W.I.P.】M2Det @2018.11 (https://arxiv.org/pdf/1811.04533.pdf)
- 【W.I.P.】Deep High-Resolution Representation Learning for Human Pose Estimation @2019.02 (https://arxiv.org/pdf/1902.09212.pdf)
- 【W.I.P.】High-Resolution Representations for Labeling Pixels and Regions @2019.04 (https://arxiv.org/pdf/1904.04514.pdf)
- 【W.I.P.】YOLO
- 【W.I.P.】DeepLab
- 【W.I.P.】FCN
- 【W.I.P.】Lraspp

## Instance segmentation

## 参考

- Semantic Segmentation
  - 概要をつかむのに非常に分かりやすい
  - https://www.youtube.com/watch?v=Eu7EKQ--Rvk&list=RDCMUCRTV5p4JsXV3YTdYpTJECRA&index=21

- Alammar氏によるTransformerのvisual解説
  - http://jalammar.github.io/illustrated-retrieval-transformer/