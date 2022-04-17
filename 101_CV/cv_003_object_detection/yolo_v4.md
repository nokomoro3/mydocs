# YOLOv4

- 題名: YOLOv4: Optimal Speed and Accuracy of Object Detection
- 論文: [https://arxiv.org/abs/2004.1093](https://arxiv.org/abs/2004.1093)

## 概要

- GPU１個で学習可能で、GPUでリアルタイム推論可能なモデルを提案。
- 例えば、1080 Tiや2080 TiのGPUを使用して、誰でも超高速で正確な物体検出器を学習させることができます。
- 近年の研究成果の中でより効果が高く計算コストが低いものを取り入れている。

## 特徴

### architecture

- summary
  - backbone: CSPDarkNet53
  - neck: SPP and PAN
  - head: YOLOv3
- backbone detail
  - Bag of Freebies
    - 済：CutMix
    - 済：Mosaic data augmentation
    - 済：DropBlock regularization
    - 済：Class label smoothing
  - Bag of Specials
    - 済：Mish activation
    - 済：CSP: Cross-stage partial connections
    - 済：MiWRC: Multi-input weighted residual connections
- detector detail
  - Bag of Freebies
    - 済：CIoU-loss
    - CmBN
    - 済：DropBlock regularization
    - 済：Mosaic data augmentation
    - Self-Adversarial Training
    - Eliminate grid sensitivity
    - Using multiple anchors for a single ground truth
    - Cosine annealing scheduler
    - Optimal hyper parameters
    - Random training shapes
  - Bag of Specials
    - 済：Mish activation
    - 済：SPP-block
    - 済：SAM-block
    - 済：PAN: path-aggregation block
    - DIoU-NMS

### Darknet-53 (backbone original)

- バックボーンそのものは、DarkNet-53となり、YOLOv3で使われている。

![](./img/yolo_v3_architecture_backbone.png)

### CSP: Cross stage partial connections

- Darknet-53にはResidualがあるため、ResNetの派生形とみなせる。
- CSPはResNet系に適用する場合も存在するため、以下のようなブロック構成で適用されていると考えられる。

![](../cv_002_classification/img/cspnet_apply_resnet.png)

### MiWRC: Multi-input weighted residual connections

- BiFPN(EfficientDet)で実施されていた、複数の入力の重み付き和のことらしい。
  - EfficientDetでは、PANetの修正版としてBiFPNを提案しているので、PANetとの両立方法が不明だが...
  - backboneの工夫としているので、neckではなくbackboneでこれを実施している??

- 以下を参考にすると、ResNetの部分での加算時に重みづけ和をしていると考えられる。
  - https://jonathan-hui.medium.com/yolov4-c9901eaa8e61

### CutMix

- Data Augmentationの手法。Cutout + Mixupの組み合わせ。
  - 論文はこちら
    - https://arxiv.org/abs/1905.04899

![](./img/yolo_v4_cutmix.png)

- ここに詳しい
  - https://qiita.com/wakame1367/items/82316feb7268e56c6161
  - Beta分布と書いてあるが、α=1のB(α,α)なので、一様分布としてよい。

- 貼り付ける場所は、元画像の場所から変更しないようだ。
  - アスペクト比が違う画像やサイズ違いはどう扱う？？

### Mosaic data augmentation

- Data Augmentationの手法。YOLOv4で提案された技術。

- CutMixは２つの画像であったが、Mosaicでは４つの画像を特定の比率で合成する。

![](./img/yolo_v4_mosaic.png)

- 通常よりも小さいオブジェクトを識別できるようになる。

- また1画像に様々な要素が入るため、ミニバッチサイズを減らすことが可能。

- 具体的には
  - ４つの画像を固定にリサイズ(416x416など)して接続。
  - 中心を基準に固定サイズ(416x416)を切り出す。
  - リサイズは歪まないようにしている可能性が高い(たぶんpaddingしている)。
  - これを参考にした。
    - https://blog.roboflow.com/yolov4-data-augmentation/

- backboneに適用する場合、ラベルを面積で重みづけしているのかな？

### DropBlock

- Dropoutの改良版。
- Dropoutは、全結合層では有効だが、Convではあまり有効ではないという報告がありました。
- その理由として、空間的に隣り合った画素にコンテキストがあるためと考えられる。
  - 完全なランダムはコンテキストを無視するため。
- そのため、特徴量マップのブロック単位でDropさせる方式がDropBlockとなる。

- 論文
  - https://arxiv.org/abs/1810.12890

- 解説
  - https://qiita.com/tan-z-tan/items/adfcfa0cf1815520560c

### Label smoothing

- 推定する教師データを0,1ではなくノイズを混ぜる。
  - この場合は、たぶんsoftmaxなしのsigmoidで、確率値の回帰問題にするのかな？

- 具体的には、正解クラスで1となるところを0.7くらいにして、残り0.3をK個のクラスで均等に割る
  - 正解クラスは 0.7 + 0.3/K となる。
  - それ以外は 0.3/K となる。

### Mish

- 比較的新しい活性化関数を使用。

- 論文はこちら
  - https://arxiv.org/abs/1908.08681

### PANet

- 各解像度の特徴量の統合にはPANetを使用している。

- 詳細は、PANetの解説を参照。

- 以下の解説によると、PANetは、接続に加算が用いられているが、YOLOv4では加算の代わりにconcatenateで実装されている
  - https://jonathan-hui.medium.com/yolov4-c9901eaa8e61
  - これはDenseNetの思想に近い。

- イメージ図は以下。

![](./img/yolo_v4_panet_of_yolo_v4.png)

- それに加えて、すべてのレイヤの情報を要素毎の最大値計算で融合する？と書いてある。

### SPP

- SPPNetで用いられた最終的なPooling処理であるが、どのように適用しているのかわかりずらい。
  - SPPNetは2stageモデルであり、RPNにより領域提案されたbboxに対してSPPを計算。
  - それをFlattenにすると固定長になるため、後段の全結合層に渡す。

- YOLOv4は1stageモデルなので、領域提案はないはず。

- なので、特徴量マップ全体について、poolingしているのではと予想される。(strideは1だしね)

- 論文には、YOLOv3の設計に従ってとあるものの、YOLOv3の論文にはこの方法について記載がない...

- poolingのkernelサイズは、YOLOv3と同じであれば、k＝{1、5、9、13}の４パターンとなる。

- ここの解説では、上記と認識が合っているようだ。
  - https://jonathan-hui.medium.com/yolov4-c9901eaa8e61

### SAM (Spatial Attention Module)

- 一般的なSAMは以下の通り
  - SAMは入力する特徴量マップに対して、max poolingとaverage poolingをそれぞれ適用する。
  - その結果を畳み込み、sigmoid関数を用いて0.0～1.0の空間方向のアテンションを作成する。
  - アテンションはチャネル数1のものとなる。

![](./img/yolo_v4_cam_and_sam_module.png)

- これを入力マップに乗算することで処理をする。

![](./img/yolo_v4_cam_and_sam_apply.png)

- これについての論文はこちら。
  - https://arxiv.org/pdf/1807.06521.pdf

- YOLOv4では、SAMのみを用い、加えてSAMをpoolingを使わないものに修正して利用する。

![](./img/yolo_v4_sam_of_yolo_v4.png)

### CIoU-loss

- そもそもbounding boxのロス関数は、x,y,w,hのL1もしくはL2ノルムとなっていた。
- これは、IoUを改善するためにはマッチしておらず、直接IoUを向上させるようなロス関数を用いるべきという提案が近年出ている。
- 通常のIoUをロスとする場合、以下のような関数がロス関数となる。

![](./img/yolo_v4_loss_normal_iou.png)

- しかしこの場合、重なってないものは常に同じロスとなる。
- 正解領域から遠いことが、ロス関数に反映されていないため、ロス関数により適切な更新をどうすべきかわからなくなる。

- これを解決するために、GIoUという方式がある。
- これはBとB_gtを含む最小のCというboxを定義して、以下のようにペナルティを計算する。

![](./img/yolo_v4_loss_giou.png)

- しかしこれに従う場合、予測するbounding boxを広げて、以下のような戦略で学習される。
  - ground truthに重なるまで広げる。
  - その後、領域を縮小し、IoUを更新する。
- そのため、学習に多くの時間が必要である。

- このため、距離に応じたDistance-IoU Loss(DIoU-loss)をまず考える。

![](./img/yolo_v4_loss_diou.png)

- ここでρ(b,bgt)は、それぞれのboxのユークリッド距離であり、cはCの対角線の長さである。
- これにより、中心の距離をロス関数に反映でき、高速に回帰が可能。
- またGIoUではIoUよりも劣化するケースがあったが、それも改善した。

- それに加え、よりよいbounding box回帰のために縦横比を考慮する。
  - 重なり面積、中心距離、縦横比の３つが幾何学要因として重要である。
  - DIoUはこのうち２つのみ考慮している。

- 以下がそのCIoUとなる。

![](./img/yolo_v4_loss_ciou.png)

- ここでvは以下のようにアスペクト比から計算される値であり、(範囲は0～1)

![](./img/yolo_v4_loss_ciou_aspect.png)

- αはIoUに応じて、アスペクト比に対する重みを調整する以下のようなパラメータである。
  - とりあえずIoUが正解から遠い場合より、近くなった場合にアスペクト比によるロスを考慮する割合が増える。

![](./img/yolo_v4_loss_ciou_aspect_coeff.png)

- これをYOLOv4で用いるが、提案している論文はこちらである。
  - https://arxiv.org/pdf/1911.08287.pdf

## 実験結果


## 参考

- CutMixについて
  - https://arxiv.org/abs/1905.04899
  - https://qiita.com/wakame1367/items/82316feb7268e56c6161

- 神解説です
  - https://blog.albert2005.co.jp/2020/01/10/advanced-technology-section-cornernet/

- soft-NMSについての解説
  - https://qiita.com/mshinoda88/items/c7e0967923e3ed47fee5

- HourglassNetwork
  - https://yusuke-ujitoko.hatenablog.com/entry/2017/07/22/000523