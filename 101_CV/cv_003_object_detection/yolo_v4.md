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
    - CutMix
    - Mosaic data augmentation
    - DropBlock regularization
    - Class label smoothing
  - Bag of Specials
    - Mish activation
    - CSP: Cross-stage partial connections
    - MiWRC: Multi-input weighted residual connections
- detector detail
  - Bag of Freebies
    - CIoU-loss
    - CmBN
    - DropBlock regularization
    - Mosaic data augmentation
    - Self-Adversarial Training
    - Eliminate grid sensitivity
    - Using multiple anchors for a single ground truth
    - Cosine annealing scheduler
    - Optimal hyper parameters
    - Random training shapes
  - Bag of Specials
    - Mish activation
    - SPP-block
    - SAM-block
    - PAN: path-aggregation block
    - DIoU-NMS

### Darknet-53 (backbone original)

- バックボーンそのものは、DarkNet-53となり、YOLOv3で使われている。

![](./img/yolo_v3_architecture_backbone.png)

### Apply CSP

- Darknet-53にはResidualがあるため、ResNetの派生形とみなせる。
- CSPはResNet系に適用する場合も存在するため、以下のようなブロック構成で適用されていると考えられる。

![](../cv_002_classification/img/cspnet_apply_resnet.png)

### MiWRC: Multi-input weighted residual connections

- BiFPN(EfficientDet)で実施されていた、複数の入力の重み付き和のことらしい。
  - EfficientDetでは、PANetの修正版としてBiFPNを提案しているので、PANetとの両立方法が不明だが...
  - backboneの工夫としているので、neckではなくbackboneでこれを実施している??

- 以下を参考にすると、ResNetの部分での加算時に重みづけ和をしていると考えられる。
  - https://jonathan-hui.medium.com/yolov4-c9901eaa8e61

## 実験結果


## 参考

- 神解説です
  - https://blog.albert2005.co.jp/2020/01/10/advanced-technology-section-cornernet/

- soft-NMSについての解説
  - https://qiita.com/mshinoda88/items/c7e0967923e3ed47fee5

- HourglassNetwork
  - https://yusuke-ujitoko.hatenablog.com/entry/2017/07/22/000523