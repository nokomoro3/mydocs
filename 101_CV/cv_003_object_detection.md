# Object detection

## 概要

- 物体検出とはclassificationに加えて、localizationが必要なタスクである。
- 物体を囲む矩形領域をbounding boxと呼ぶ。
- 物体検出は以下を予測値として学習する。
  - classification
  - 物体の中心座標x,y(連続値)
  - bounding boxのheightとwidth(連続値)
- 学習の際、正解がない場所についてはback propagationをしないなど工夫をして学習が必要。

- 上記の基本的な手法のままでは、以下の課題がある
  - Convの最終層の画像サイズに依存した個数しか検出できない。(最終層が8x8ならば64個が上限)
  - 中心が同じ領域にあるものを検出できない。(学習時の正解も一つしか設定できない)

- 上記を解決するために、anchor boxを準備する。
  - anchor boxはいろんなサイズ、縦横比のbounding box候補のようなイメージ。
  - anchor boxの種類毎に、上記の基本的な手法で学習を実施する。
  - anchor boxの種類はクラスタリングなどにより求める。
  - anchor boxの種類毎に、最終層の出力解像度を分けるアプローチのモデルもある。

- NMS(Non-maximum Supressions)で候補を削減。
  - anchor boxを用いると、重なりの大きな物体が複数検出されるので、IoUが一定以上の場合は、その中で一番大きいスコアの領域のみを採用する。

- これらの手順は煩雑なので、anchor boxを使わない方法も近年人気である。
  - 最終層を元画像の1/4程度に収めて、高解像で出力するCenterNetなど。

## 関連学会

- CVPR
- ICCV
- NeurlPS

## レポジトリ(フレームワーク)

- [detectron2](https://github.com/facebookresearch/detectron2)
  - FAIRによるレポジトリ
- [mmdetection](https://github.com/open-mmlab/mmdetection)
  - OpenMMLabによるレポジトリ
- [PaddleDetection](https://github.com/PaddlePaddle/PaddleDetection)
  - Baidu社によるPaddlePaddleというフレームワークを用いたレポジトリ

## 主要なモデル一覧

|名前|発表年月日|サマリ|カテゴリ|ベースCNN|解説|元論文|実装|
|:---|:---|:---|:---|:---|:---|:---|:---|
|HOG+SVM|2005/06/20|・CNN誕生前のモデルで良く参照された論文<br>・HOGはpixel変化を捉える特徴量<br>・HOG特徴量を用いてSVMで多クラス分類する<br>・NMSはこの時から使われている|None|None|[解説](./cv_003_object_detection/hog_svm.md)|[論文](http://lear.inrialpes.fr/people/triggs/pubs/Dalal-cvpr05.pdf)|None|
|R-CNN|2013/11/11|・CNN適用の先駆け論文<br>・selective searchという古典的手法で領域候補を抽出<br>・候補領域をリサイズしてCNNに入力して特徴量ベクトルを得る<br>・その後段で1-class SVMとbounding boxのregressionを実施|2stage|AlexNet<br>VGG16|[解説](./cv_003_object_detection/r_cnn.md)|[論文](https://arxiv.org/abs/1311.2524)|[公式(MATLAB)](https://github.com/rbgirshick/rcnn)<br>[paperswithcode](https://paperswithcode.com/paper/rich-feature-hierarchies-for-accurate-object)|
|SPP-net|2014/06/18|・領域候補の切り出しを特徴量マップに対して実施し効率化<br>・これにより最大2000毎程度ある領域候補のCNN処理が1回で実現可能<br>・切り出した特徴量マップはSPPで固定長の特徴量に変換して後段で処理<br>|2stage|ZFNet<br>AlexNet<br>OverFeat|[解説](./cv_003_object_detection/spp_net.md)|[論文](https://arxiv.org/abs/1406.4729)|[paperswithcode](https://paperswithcode.com/paper/spatial-pyramid-pooling-in-deep-convolutional)|
|Fast R-CNN|2015/04/30|・Multi-task lossによりclassificationとbounding box推定を同時学習<br>・固定長の特徴量変換としてSPPの代わりにRoI Poolingを使用<br>・RoI Poolingは両機をグリッド分割し各グリッドに対してpooling処理を実施|2stage|VGG16|[解説](cv_003_object_detection/fast_r_cnn.md)|[論文](https://arxiv.org/abs/1504.08083)|[公式(Caffe2)](https://github.com/rbgirshick/fast-rcnn)<br>[paperswithcode](https://paperswithcode.com/paper/fast-r-cnn)|
|Faster R-CNN|2015/06/04|・領域候補を抽出するselective searchをCNNで置き換え<br>・このNetworkをRPN(Region Proposal Network)と呼ぶ<br>・RPNには9個のanchor boxを使用し領域有無とbounding box回帰を実施|2stage|VGG16<br>ResNet101|[解説](./cv_003_object_detection/faster_r_cnn.md)|[論文](https://arxiv.org/abs/1506.01497)|[公式(Caffe2)](https://github.com/rbgirshick/py-faster-rcnn)<br>[paperswithcode](https://paperswithcode.com/paper/faster-r-cnn-towards-real-time-object)|
|YOLO|2015/06/08|・領域検出とclassificationの同時学習を実現(You only look once)<br>・入力画像をグリッド分割し、各グリッドに固定数のbounding box回帰を実施|1stage<br>(anchor free)|独自実装|[解説](./cv_003_object_detection/yolo.md)|[論文](https://arxiv.org/abs/1506.02640)|[公式(Darknet)](https://pjreddie.com/darknet/yolo)<br>[paperswithcode](https://paperswithcode.com/paper/you-only-look-once-unified-real-time-object)|
|SSD|2015/12/08|・YOLOと同じく領域検出とclassificationの同時学習を実現(Single shot detector)<br>・backboneの途中層の高解像な特徴量マップを使用<br>・特徴量マップの解像度毎に様々な大きさのanchor boxを定義|1stage<br>(anchor box)|VGG16|[解説](./cv_003_object_detection/ssd.md)|[論文](https://arxiv.org/abs/1512.02325)|[公式(Caffe)](https://github.com/weiliu89/caffe/tree/ssd)<br>[paperswithcode](https://paperswithcode.com/paper/ssd-single-shot-multibox-detector)|
|R-FCN|2016/05/20|・RoI Pooling以降の全結合層をCNN化したモデル<br>・RoI Poolingの前段にposition sensitive mapという畳み込みを追加<br>・その後段でRoI Poolingを実施する構成|2stage|ResNet-101|[解説](./cv_003_object_detection/r_fcn.md)|[論文](https://arxiv.org/abs/1605.06409)|[公式(MXNet)](https://github.com/daijifeng001/r-fcn)<br>[paperswithcode](https://paperswithcode.com/paper/r-fcn-object-detection-via-region-based-fully)|
|FPN|2016/12/09|・ピラミッド構造の特徴量マップを実現したモデル<br>・ベースはFaster R-CNNであり、RPNとclassificationどちらもピラミッド構造化<br>・FPNの考え方は現在まで採用されており重要な論文|2stage|ResNet-50<br>ResNet-101|[解説](./cv_003_object_detection/fpn.md)|[論文](https://arxiv.org/abs/1612.03144)|[paperswithcode](https://paperswithcode.com/paper/feature-pyramid-networks-for-object-detection)|
|YOLOv2(YOLO9000)|2016/12/25|・YOLOをベースに改良を実施<br>・入力の高解像化、anchor boxの導入、高解像な特徴量マップの使用など<br>・それ以外に9000クラスまで分類可能なWordTreeというアーキテクチャを構築|1stage<br>(anchor box)|Darknet-19|[解説](./cv_003_object_detection/yolo_v2.md)|[論文](https://arxiv.org/abs/1612.08242)|[公式(Darknet)](https://pjreddie.com/darknet/yolo)<br>[paperswithcode](https://paperswithcode.com/paper/yolo9000-better-faster-stronger)|
|RetinaNet|2017/08/07|・ロス計算(CE)をサンプルの難易度によって動的に変化させるFocal lossを提案<br>・ベース構造はFPNのようなピラミッド構成だが、1stageとなっている。<br>・シンプルな構成のため現在でもベースラインされることも多い<br>・Focal loss自体も現在でも使用されている重要な論文|1stage<br>(anchor box)|ResNet-50<br>ResNet-101|[解説](./cv_003_object_detection/retinanet.md)|[論文](https://arxiv.org/abs/1708.02002)|[公式(detectron)](https://github.com/facebookresearch/Detectron)<br>[paperswithcode](https://paperswithcode.com/paper/focal-loss-for-dense-object-detection)|
|Mask R-CNN|2017/03/20|・Instance Segmentation用のheadを追加したMulti-taskモデル<br>・物体検知としては、RoI Poolingの代わりに補完を考慮したRoI Alignを導入|2stage|ResNet-50<br>ResNet-101<br>ResNeXt|[解説](./cv_003_object_detection/mask_r_cnn.md)|[論文](https://arxiv.org/abs/1703.06870)|[公式(tensorflow)](https://github.com/matterport/Mask_RCNN)<br>[paperswithcode](https://paperswithcode.com/paper/mask-r-cnn)|
|RefineDet|2017/11/18|・1stageモデルだが2stageモデルのようにanchor boxをCNNで推定<br>・anchor box用CNNと物体検知用のCNNを相互接続し同時に学習<br>・改良のベースはSSDを用いているが、近年参照されることは少ない|1stage<br>(anchor free)| VGG16<br>ResNet-101|[解説](./cv_003_object_detection/refinedet.md)|[論文](https://arxiv.org/abs/1711.06897)|[公式(Caffe)](https://github.com/sfzhang15/RefineDet)<br>[paperswithcode](https://paperswithcode.com/paper/single-shot-refinement-neural-network-for)|
|PANet|2018/03/05|・buttom-up構造により深い特徴量マップにも高解像情報を付与<br>・後段はMask R-CNNと同様のRoI Alignを使用している<br>・RPN側は従来通りのFPN構造と思われる<br>・Mask R-CNNと同様、Segmentationタスクにも対応したMulti-taskモデル|2stage|ResNet-50<br>ResNeXt-101|[解説](./cv_003_object_detection/panet.md)|[論文](https://arxiv.org/abs/1803.01534)|[公式(PyTorch)](https://github.com/ShuLiu1993/PANet)<br>[paperswithcode](https://paperswithcode.com/paper/path-aggregation-network-for-instance)|
|YOLOv3|2018/04/08|・YOLOv2をベースに改良を実施<br>・skip-connectionの採用、複数解像度(3つ)の特徴量の融合など実施<br>・anchor box数を増やし、結果として速度面ではYOLOv2よりも低下|1stage<br>(anchor box)|Darknet-53|[解説](./cv_003_object_detection/yolo_v3.md)|[論文](https://arxiv.org/abs/1804.02767)|[公式(Darknet)](https://pjreddie.com/darknet/yolo/)<br>[paperswithcode](https://paperswithcode.com/paper/yolov3-an-incremental-improvement)|
|CornetNet|2018/08/03|・姿勢推定の影響を受けた技術が多いanchor freeの方式<br>・corner poolingによりbounding boxのコーナーをうまく検出している<br>・またImageNetなどの事前学習が不要な点も特徴的|1stage<br>(anchor free)|Hourglass104|[解説](./cv_003_object_detection/cornernet.md)|[論文](https://arxiv.org/abs/1808.01244)|[公式(PyTorch)](https://github.com/princeton-vl/CornerNet)<br>[paperswithcode](https://paperswithcode.com/paper/cornernet-detecting-objects-as-paired)|
|M2Det|2018/11/12|・SSDをベースに特徴量マップを複雑に処理するMLFPNを提案<br>・MLFPNはFPNのピラミッド構造を繰り返し可能な設計とした構造<br>・当時は高速・高性能をうたっていたが近年参照されることは少ない論文|1stage<br>(anchor box)|VGG16<br>ResNet-101|[解説](./cv_003_object_detection/m2det.md)|[論文](https://arxiv.org/abs/1811.04533)|[公式(PyTorch)](https://github.com/VDIGPKU/M2Det)<br>[paperswithcode](https://paperswithcode.com/paper/m2det-a-single-shot-object-detector-based-on)|
|FCOS|2019/04/02|・物体検出をsegmentationと同様画素単位で再構築したanchor freeモデル<br>・特徴量マップの各位置からbounding boxまでの4方向の距離を直接推定する<br>・特徴量マップはFPN構造を使い複数の解像度を使用<br>・応用がしやすいため参照されることが多いanchor freeモデル|1stage<br>(anchor free)|ResNet-101<br>HRNet<br>ResNeXt-101|[解説](./cv_003_object_detection/fcos.md)|[論文](https://arxiv.org/abs/1904.01355)|[公式(PyTorch)](https://github.com/tianzhi0549/FCOS)<br>[paperswithcode](https://paperswithcode.com/paper/fcos-fully-convolutional-one-stage-object)|
|CenterNet|2019/04/16|・CornerNetと同様にheatmapを推定するanchor freeモデル<br>・CenterNetでは中心位置のheatmapを推定する<br>・高解像な特徴量を扱うための工夫がbackboneに施されている|1stage<br>(anchor free)|ResNet-18<br>ResNet-101<br>DLA-34<br>Hourglass-104|[解説](./cv_003_object_detection/centernet.md)|[論文](https://arxiv.org/abs/1904.07850)|[公式(PyTorch)](https://github.com/xingyizhou/CenterNet)<br>[paperswithcode](https://paperswithcode.com/paper/objects-as-points)|
|CenterNet|2019/04/17|・CenterNetは同名のものが２つあるがこちらはCornerNetの改良版<br>・ConerNetはbounding boxの内部を見ていないため誤検出が多い<br>・改善するためにCenter heatmapを計算し、Corner Poolingも改良|1stage<br>(anchor free)|Hourglass-52<br>Hourglass-104|[解説](./cv_003_object_detection/centernet2.md)|[論文](https://arxiv.org/abs/1904.08189)|[公式(PyTorch)](https://github.com/Duankaiwen/CenterNet)<br>[paperswithcode](https://paperswithcode.com/paper/centernet-object-detection-with-keypoint)|
|EfficientDet|2019/11/20|・EfficientNetの考え方を取り入れた複合スケール方式を提案<br>・backbone自体もEfficientNetを使用<br>・FPN系のアーキテクチャの体系を整理しより最適でスケーラブルなBiFPNを提案<br>・Focal Lossを用いるなどベースの部分はRetinaNetに近いと思われる|1stage<br>(anchor box)|EfficientNet|[解説](./cv_003_object_detection/efficientdet.md)|[論文](https://arxiv.org/abs/1911.09070)|[公式(tensorflow)](https://github.com/google/automl)<br>[paperswithcode](https://paperswithcode.com/paper/efficientdet-scalable-and-efficient-object)|
|YOLOv4|2020/04/23|・研究成果を体系的に整理し高速かつ高精度なYOLOを目指した改良版<br>・GPU1個で学習可能でリアルタイム推論が可能なモデルを構築|1stage<br>(anchor box)|CSPDarknet-53|[解説](./cv_003_object_detection/yolo_v4.md)|[論文](https://arxiv.org/abs/2004.10934)|[公式(tensorflow)](https://github.com/AlexeyAB/darknet)<br>[paperswithcode](https://paperswithcode.com/paper/yolov4-optimal-speed-and-accuracy-of-object)|
|DETR|2020/05/26|・Transformerを物体検知に導入したモデル<br>・backboneの後段の処理としてTransformerを使用<br>・これによりanchor boxやNMSなどハンドメイドな部分を削除<br>・正解の割り当てを最適割当問題とみなしhungarian algorithmで解く<br>・比較対象がFaster R-CNNなのでこれからの発展に期待|1stage<br>(anchor free)<br>Transformer|ResNet-50<br>ResNet-101|[解説](./cv_003_object_detection/detr.md)|[論文](https://arxiv.org/abs/2005.12872)|[公式(PyTorch)](https://github.com/facebookresearch/detr)<br>[paperswithcode](https://paperswithcode.com/paper/end-to-end-object-detection-with-transformers)|
|YOLOv5|2020/06/01|・フレームワークが優れており、広く利用されている物体検知のモデル<br>・アーキテクチャの詳細は論文がないため確認が困難<br>・v4との比較について第三者が検証しているが明確な結論はでていない|1stage<br>(ancor box)|???|[解説](./cv_003_object_detection/yolo_v5.md)|なし|[公式(PyTorch)](https://github.com/ultralytics/yolov5)|
|PP-YOLO|2020/07/23|未調査|未調査|未調査|未調査|[論文](https://arxiv.org/abs/2007.12099)|[公式(Paddle)](https://github.com/PaddlePaddle/PaddleDetection)<br>[paperswithcode](https://paperswithcode.com/paper/pp-yolo-an-effective-and-efficient)|
|PSS|2021/01/28|WIP|WIP|WIP|[解説](./cv_003_object_detection/pss.md)|[論文](https://arxiv.org/abs/2101.11782)|[公式(PyTorch)](https://github.com/damo-cv/FCOSPss)<br>[paperswithcode](https://paperswithcode.com/paper/object-detection-made-simpler-by-eliminating)|
|YOLOF|2021/03/17|WIP|WIP|ResNet-50<br>ResNet-101|[解説](./cv_003_object_detection/yolo_f.md)|[論文](https://arxiv.org/abs/2103.09460)|[公式(PyTorch)](https://github.com/megvii-model/YOLOF)<br>[paperswithcode](https://paperswithcode.com/paper/you-only-look-one-level-feature)|
|Swin Transformer|2021/03/25|未調査|未調査|未調査|未調査|[論文](https://arxiv.org/abs/2103.14030)|[公式(PyTorch)](https://github.com/SwinTransformer/Swin-Transformer-Object-Detection)<br>[paperswithcode](https://paperswithcode.com/paper/swin-transformer-hierarchical-vision)|
|OTA|2021/03/26|WIP|WIP|WIP|[解説](./cv_003_object_detection/ota.md)|[論文](https://arxiv.org/abs/2103.14259)|[公式(PyTorch)](https://github.com/Megvii-BaseDetection/OTA)<br>[paperswithcode](https://paperswithcode.com/paper/ota-optimal-transport-assignment-for-object)|
|PP-YOLOv2|2021/04/21|未調査|未調査|未調査|未調査|[論文](https://arxiv.org/abs/2104.10419)|[公式(Paddle)](https://github.com/PaddlePaddle/PaddleDetection)<br>[paperswithcode](https://paperswithcode.com/paper/pp-yolov2-a-practical-object-detector)|
|YOLOX|2021/07/18|WIP|WIP|WIP|[解説](./cv_003_object_detection/yolo_x.md)|[論文](https://arxiv.org/abs/2107.08430)|[公式(PyTorch)](https://github.com/Megvii-BaseDetection/YOLOX)<br>[paperswithcode](https://paperswithcode.com/paper/yolox-exceeding-yolo-series-in-2021)|
|YOLOP|2021/08/25|未調査|未調査|未調査|未調査|[論文](https://arxiv.org/abs/2108.11250)|[公式(PyTorch)](https://github.com/hustvl/yolop)<br>[paperswithcode](https://paperswithcode.com/paper/yolop-you-only-look-once-for-panoptic-driving)|
|PP-PicoDet|2021/11/01|未調査|未調査|未調査|未調査|[論文](https://arxiv.org/abs/2111.00902)|[公式(Paddle)](https://github.com/PaddlePaddle/PaddleDetection)<br>[paperswithcode](https://paperswithcode.com/paper/pp-picodet-a-better-real-time-object-detector)|
|PP-YOLOE|2022/03/30|未調査|未調査|未調査|未調査|[論文](https://arxiv.org/abs/2203.16250)|[公式(Paddle)](https://github.com/PaddlePaddle/PaddleDetection)<br>[paperswithcode](https://paperswithcode.com/paper/pp-yoloe-an-evolved-version-of-yolo)|
|ViTDet|2022/03/30|未調査|未調査|未調査|未調査|[論文](https://arxiv.org/abs/2203.16527)|[公式(PyTorch)](https://github.com/ViTAE-Transformer/ViTDet)<br>[paperswithcode](https://paperswithcode.com/paper/exploring-plain-vision-transformer-backbones)|

## 参考

- 物体検出の概要がわかる
  - https://www.youtube.com/watch?v=5nmVHoA-A2E

- 物体検出についての歴史まとめ
  - https://qiita.com/mshinoda88/items/9770ee671ea27f2c81a9
  - https://qiita.com/mshinoda88/items/c7e0967923e3ed47fee5

- 物体検出モデルの進展
  - https://qiita.com/TaigaHasegawa/items/b05110a2571a5289cbab
  - https://qiita.com/TaigaHasegawa/items/a3cb98fb27cc7a9307b4
  - https://qiita.com/TaigaHasegawa/items/653abc81ac4ee1f0d7b8

- YOLOシリーズ徹底解説
  - https://deepsquare.jp/2020/09/yolo/

- ディープラーニングによる一般物体検出アルゴリズムまとめ | NegativeMindException
  - https://blog.negativemind.com/portfolio/deep-learning-generic-object-detection-algorithm/

- YOLOのv1～v5まで
  - https://qiita.com/tfukumori/items/519d84bf3feb8d246924

- YOLOシリーズについて2021年時点までの情報がいろいろ書いてある。
  - https://www.slideshare.net/ren4yu/you-only-look-onelevel-feature

- 2014～2019までの物体検知の歴史
  - https://github.com/hoya012/deep_learning_object_detection
