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

- まとめ
  - https://aru47.hatenablog.com/entry/2022/01/01/123929

- [mmdetection](https://github.com/open-mmlab/mmdetection)
  - OpenMMLabによるレポジトリ
- [PaddleDetection](https://github.com/PaddlePaddle/PaddleDetection)
  - Baidu社によるPaddlePaddleというフレームワークを用いたレポジトリ
- [detectron2](https://github.com/facebookresearch/detectron2)
  - FAIRによるレポジトリ

## 主要なモデル一覧

<table>
  <thead><tr><th>名前</th><th>発表年月日</th><th>サマリ</th><th>カテゴリ</th><th>backbone</th><th>リンク</th></tr></thead>
  <tbody>
    <tr>
      <td>HOG+SVM</td><td>2005/06/20</td>
      <td>
        ・CNN誕生前のモデルで良く参照された論文<br>
        ・HOGはpixel変化を捉える特徴量<br>
        ・HOG特徴量を用いてSVMで多クラス分類する<br>
        ・NMSはこの時から使われている
      </td>
      <td>None</td><td>None</td>
      <td>
        <a href="./cv_003_object_detection/hog_svm.md">解説</a><br>
        <a href="http://lear.inrialpes.fr/people/triggs/pubs/Dalal-cvpr05.pdf">論文</a>
      </td>
    </tr>
    <tr>
      <td>R-CNN</td><td>2013/11/11</td>
      <td>
        ・CNN適用の先駆け論文<br>
        ・selective searchという古典的手法で領域候補を抽出<br>
        ・候補領域をリサイズしてCNNに入力して特徴量ベクトルを得る<br>
        ・その後段で1-class SVMとbounding boxのregressionを実施
      </td>
      <td>2stage</td><td>AlexNet<br>VGG16</td>
      <td>
        <a href="./cv_003_object_detection/r_cnn.md">解説</a><br>
        <a href="https://arxiv.org/abs/1311.2524">論文</a><br>
        <a href="https://paperswithcode.com/paper/rich-feature-hierarchies-for-accurate-object">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>SPP-net</td><td>2014/06/18</td>
      <td>
        ・領域候補の切り出しを特徴量マップに対して実施し効率化<br>
        ・これにより最大2000毎程度ある領域候補のCNN処理が1回で実現可能<br>
        ・切り出した特徴量マップはSPPで固定長の特徴量に変換して後段で処理
      </td>
      <td>2stage</td><td>ZFNet<br>AlexNet<br>OverFeat</td>
      <td>
        <a href="./cv_003_object_detection/spp_net.md">解説</a><br>
        <a href="https://arxiv.org/abs/1406.4729">論文</a><br>
        <a href="https://paperswithcode.com/paper/spatial-pyramid-pooling-in-deep-convolutional">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>Fast R-CNN</td><td>2015/04/30</td>
      <td>
        ・Multi-task lossによりclassificationとbounding box推定を同時学習<br>
        ・固定長の特徴量変換としてSPPの代わりにRoI Poolingを使用<br>
        ・RoI Poolingは両機をグリッド分割し各グリッドに対してpooling処理を実施
      </td>
      <td>2stage</td><td>VGG16</td>
      <td>
        <a href="cv_003_object_detection/fast_r_cnn.md">解説</a><br>
        <a href="https://arxiv.org/abs/1504.08083">論文<br>
        <a href="https://paperswithcode.com/paper/fast-r-cnn">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>Faster R-CNN</td><td>2015/06/04</td>
      <td>
        ・領域候補を抽出するselective searchをCNNで置き換え<br>
        ・このNetworkをRPN(Region Proposal Network)と呼ぶ<br>
        ・RPNには9個のanchor boxを使用し領域有無とbounding box回帰を実施
      </td>
      <td>2stage</td><td>VGG16<br>ResNet101</td>
      <td>
        <a href="./cv_003_object_detection/faster_r_cnn.md">解説</a><br>
        <a href="https://arxiv.org/abs/1506.01497">論文</a><br>
        <a href="https://paperswithcode.com/paper/faster-r-cnn-towards-real-time-object">paperswithcode</a>
      <td>
    </tr>
    <tr>
      <td>YOLO</td><td>2015/06/08</td>
      <td>
        ・領域検出とclassificationの同時学習を実現(You only look once)<br>
        ・入力画像をグリッド分割し、各グリッドに固定数のbounding box回帰を実施
      </td>
      <td>1stage<br>(anchor free)</td><td>独自実装</td>
      <td>
        <a href="./cv_003_object_detection/yolo.md">解説</a><br>
        <a href="https://arxiv.org/abs/1506.02640">論文</a><br>
        <a href="https://paperswithcode.com/paper/you-only-look-once-unified-real-time-object">paperswithcode</a>
      <td>
    </tr>
    <tr>
      <td>SSD</td><td>2015/12/08</td>
      <td>
        ・YOLOと同じく領域検出とclassificationの同時学習を実現(Single shot detector)<br>
        ・backboneの途中層の高解像な特徴量マップを使用<br>
        ・特徴量マップの解像度毎に様々な大きさのanchor boxを定義
      </td>
      <td>1stage<br>(anchor box)</td><td>VGG16</td>
      <td>
        <a href="./cv_003_object_detection/ssd.md">解説</a><br>
        <a href="https://arxiv.org/abs/1512.02325">論文</a><br>
        <a href="https://paperswithcode.com/paper/ssd-single-shot-multibox-detector">paperswithcode</a>
      <td>
    </tr>
    <tr>
      <td>R-FCN</td><td>2016/05/20</td>
      <td>
        ・RoI Pooling以降の全結合層をCNN化したモデル<br>
        ・RoI Poolingの前段にposition sensitive mapという畳み込みを追加<br>
        ・その後段でRoI Poolingを実施する構成
      </td>
      <td>2stage</td><td>ResNet-101</td>
      <td>
        <a href="./cv_003_object_detection/r_fcn.md">解説</a><br>
        <a href="https://arxiv.org/abs/1605.06409">論文</a><br>
        <a href="https://paperswithcode.com/paper/r-fcn-object-detection-via-region-based-fully">paperswithcode</a>
      <td>
    </tr>
    <tr>
      <td>FPN</td><td>2016/12/09</td>
      <td>
        ・ピラミッド構造の特徴量マップを実現したモデル<br>
        ・ベースはFaster R-CNNであり、RPNとclassificationどちらもピラミッド構造化<br>
        ・FPNの考え方は現在まで採用されており重要な論文
      </td>
      <td>2stage</td><td>ResNet-50<br>ResNet-101</td>
      <td>
        <a href="./cv_003_object_detection/fpn.md">解説</a><br>
        <a href="https://arxiv.org/abs/1612.03144">論文</a><br>
        <a href="https://paperswithcode.com/paper/feature-pyramid-networks-for-object-detection">paperswithcode</a>
      <td>
    </tr>
    <tr>
      <td>YOLOv2(YOLO9000)</td><td>2016/12/25</td>
      <td>
        ・YOLOをベースに改良を実施<br>
        ・入力の高解像化、anchor boxの導入、高解像な特徴量マップの使用など<br>
        ・それ以外に9000クラスまで分類可能なWordTreeというアーキテクチャを構築
      </td>
      <td>1stage<br>(anchor box)</td><td>Darknet-19</td>
      <td>
        <a href="./cv_003_object_detection/yolo_v2.md">解説</a><br>
        <a href="https://arxiv.org/abs/1612.08242">論文</a><br>
        <a href="https://paperswithcode.com/paper/yolo9000-better-faster-stronger">paperswithcode</a>
      <td>
    </tr>
    <tr>
      <td>RetinaNet</td><td>2017/08/07</td>
      <td>
        ・ロス計算(CE)をサンプルの難易度によって動的に変化させるFocal lossを提案<br>
        ・ベース構造はFPNのようなピラミッド構成だが、1stageとなっている。<br>
        ・シンプルな構成のため現在でもベースラインされることも多い<br>
        ・Focal loss自体も現在でも使用されている重要な論文
      </td>
      <td>1stage<br>(anchor box)</td><td>ResNet-50<br>ResNet-101</td>
      <td>
        <a href="./cv_003_object_detection/retinanet.md">解説</a><br>
        <a href="https://arxiv.org/abs/1708.02002">論文</a><br>
        <a href="https://paperswithcode.com/paper/focal-loss-for-dense-object-detection">paperswithcode</a>
      <td>
    </tr>
    <tr>
      <td>Mask R-CNN</td><td>2017/03/20</td>
      <td>
        ・Instance Segmentation用のheadを追加したMulti-taskモデル<br>
        ・物体検知としては、RoI Poolingの代わりに補完を考慮したRoI Alignを導入
      </td>
      <td>2stage</td><td>ResNet-50<br>ResNet-101<br>ResNeXt</td>
      <td>
        <a href="./cv_003_object_detection/mask_r_cnn.md">解説</a><br>
        <a href="https://arxiv.org/abs/1703.06870">論文</a><br>
        <a href="https://paperswithcode.com/paper/mask-r-cnn">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>RefineDet</td><td>2017/11/18</td>
      <td>
        ・1stageモデルだが2stageモデルのようにanchor boxをCNNで推定<br>
        ・anchor box用CNNと物体検知用のCNNを相互接続し同時に学習<br>
        ・改良のベースはSSDを用いているが、近年参照されることは少ない
      </td>
      <td>1stage<br>(anchor free)</td><td>VGG16<br>ResNet-101</td>
      <td>
        <a href="./cv_003_object_detection/refinedet.md">解説</a><br>
        <a href="https://arxiv.org/abs/1711.06897">論文</a><br>
        <a href="https://paperswithcode.com/paper/single-shot-refinement-neural-network-for">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>PANet</td><td>2018/03/05</td>
      <td>
        ・buttom-up構造により深い特徴量マップにも高解像情報を付与<br>
        ・後段はMask R-CNNと同様のRoI Alignを使用している<br>
        ・RPN側は従来通りのFPN構造と思われる<br>
        ・Mask R-CNNと同様、Segmentationタスクにも対応したMulti-taskモデル
      </td>
      <td>2stage</td><td>ResNet-50<br>ResNeXt-101</td>
      <td>
        <a href="./cv_003_object_detection/panet.md">解説</a><br>
        <a href="https://arxiv.org/abs/1803.01534">論文</a><br>
        <a href="https://paperswithcode.com/paper/path-aggregation-network-for-instance">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>YOLOv3</td><td>2018/04/08</td>
      <td>
        ・YOLOv2をベースに改良を実施<br>
        ・skip-connectionの採用、複数解像度(3つ)の特徴量の融合など実施<br>
        ・anchor box数を増やし、結果として速度面ではYOLOv2よりも低下
      </td>
      <td>1stage<br>(anchor box)</td><td>Darknet-53</td>
      <td>
        <a href="./cv_003_object_detection/yolo_v3.md">解説</a><br>
        <a href="https://arxiv.org/abs/1804.02767">論文</a><br>
        <a href="https://paperswithcode.com/paper/yolov3-an-incremental-improvement">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>CornetNet</td><td>2018/08/03</td>
      <td>
        ・姿勢推定の影響を受けた技術が多いanchor freeの方式<br>
        ・corner poolingによりbounding boxのコーナーをうまく検出している<br>
        ・またImageNetなどの事前学習が不要な点も特徴的
      </td>
      <td>1stage<br>(anchor free)</td><td>Hourglass104</td>
      <td>
        <a href="./cv_003_object_detection/cornernet.md">解説</a><br>
        <a href="https://arxiv.org/abs/1808.01244">論文</a><br>
        <a href="https://paperswithcode.com/paper/cornernet-detecting-objects-as-paired">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>M2Det</td><td>2018/11/12</td>
      <td>
        ・SSDをベースに特徴量マップを複雑に処理するMLFPNを提案<br>
        ・MLFPNはFPNのピラミッド構造を繰り返し可能な設計とした構造<br>
        ・当時は高速・高性能をうたっていたが近年参照されることは少ない論文
      </td>
      <td>1stage<br>(anchor box)</td><td>VGG16<br>ResNet-101</td>
      <td>
        <a href="./cv_003_object_detection/m2det.md">解説</a><br>
        <a href="https://arxiv.org/abs/1811.04533">論文</a><br>
        <a href="https://paperswithcode.com/paper/m2det-a-single-shot-object-detector-based-on">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>FCOS</td><td>2019/04/02</td>
      <td>
        ・物体検出をsegmentationと同様画素単位で再構築したanchor freeモデル<br>
        ・特徴量マップの各位置からbounding boxまでの4方向の距離を直接推定する<br>
        ・特徴量マップはFPN構造を使い複数の解像度を使用<br>
        ・応用がしやすいため参照されることが多いanchor freeモデル
      </td>
      <td>1stage<br>(anchor free)</td><td>ResNet-101<br>HRNet<br>ResNeXt-101</td>
      <td>
        <a href="./cv_003_object_detection/fcos.md">解説</a><br>
        <a href="https://arxiv.org/abs/1904.01355">論文</a><br>
        <a href="https://paperswithcode.com/paper/fcos-fully-convolutional-one-stage-object">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>CenterNet</td><td>2019/04/16</td>
      <td>
        ・CornerNetと同様にheatmapを推定するanchor freeモデル<br>
        ・CenterNetでは中心位置のheatmapを推定する<br>
        ・高解像な特徴量を扱うための工夫がbackboneに施されている
      </td>
      <td>1stage<br>(anchor free)</td><td>ResNet-18<br>ResNet-101<br>DLA-34<br>Hourglass-104</td>
      <td>
        <a href="./cv_003_object_detection/centernet.md">解説</a><br>
        <a href="https://arxiv.org/abs/1904.07850">論文</a><br>
        <a href="https://paperswithcode.com/paper/objects-as-points">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>CenterNet</td><td>2019/04/17</td>
      <td>
        ・CenterNetは同名のものが２つあるがこちらはCornerNetの改良版<br>
        ・ConerNetはbounding boxの内部を見ていないため誤検出が多い<br>
        ・改善するためにCenter heatmapを計算し、Corner Poolingも改良
      </td>
      <td>1stage<br>(anchor free)</td><td>Hourglass-52<br>Hourglass-104</td>
      <td>
        <a href="./cv_003_object_detection/centernet2.md">解説</a><br>
        <a href="https://arxiv.org/abs/1904.08189">論文</a><br>
        <a href="https://paperswithcode.com/paper/centernet-object-detection-with-keypoint">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>EfficientDet</td><td>2019/11/20</td>
      <td>
        ・EfficientNetの考え方を取り入れた複合スケール方式を提案<br>
        ・backbone自体もEfficientNetを使用<br>
        ・FPN系のアーキテクチャの体系を整理しより最適でスケーラブルなBiFPNを提案<br>
        ・Focal Lossを用いるなどベースの部分はRetinaNetに近いと思われる
      </td>
      <td>1stage<br>(anchor box)</td><td>EfficientNet</td>
      <td>
        <a href="./cv_003_object_detection/efficientdet.md">解説</a><br>
        <a href="https://arxiv.org/abs/1911.09070">論文</a><br>
        <a href="https://paperswithcode.com/paper/efficientdet-scalable-and-efficient-object">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>YOLOv4</td><td>2020/04/23</td>
      <td>
        ・研究成果を体系的に整理し高速かつ高精度なYOLOを目指した改良版<br>
        ・GPU1個で学習可能でリアルタイム推論が可能なモデルを構築
      </td>
      <td>1stage<br>(anchor box)</td><td>CSPDarknet-53</td>
      <td>
        <a href="./cv_003_object_detection/yolo_v4.md">解説</a><br>
        <a href="https://arxiv.org/abs/2004.10934">論文</a><br>
        <a href="https://paperswithcode.com/paper/yolov4-optimal-speed-and-accuracy-of-object">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>DETR</td><td>2020/05/26</td>
      <td>
        ・Transformerを物体検知に導入したモデル<br>
        ・backboneの後段の処理としてTransformerを使用<br>
        ・これによりanchor boxやNMSなどハンドメイドな部分を削除<br>
        ・正解の割り当てを最適割当問題とみなしhungarian algorithmで解く<br>
        ・比較対象がFaster R-CNNなのでこれからの発展に期待
      </td>
      <td>1stage<br>(anchor free)<br>Transformer</td><td>ResNet-50<br>ResNet-101</td>
      <td>
        <a href="./cv_003_object_detection/detr.md">解説</a><br>
        <a href="https://arxiv.org/abs/2005.12872">論文</a><br>
        <a href="https://paperswithcode.com/paper/end-to-end-object-detection-with-transformers">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>YOLOv5</td><td>2020/06/01</td>
      <td>
        ・フレームワークが優れており、広く利用されている物体検知のモデル<br>
        ・アーキテクチャの詳細は論文がないため確認が困難<br>
        ・v4との比較について第三者が検証しているが明確な結論はでていない
      </td>
      <td>1stage<br>(ancor box)</td><td>???</td>
      <td>
        <a href="./cv_003_object_detection/yolo_v5.md">解説</a><br>
        <a href="https://github.com/ultralytics/yolov5">公式(PyTorch)</a>
      </td>
    </tr>
    <tr>
      <td>PP-YOLO</td><td>2020/07/23</td>
      <td>
        未調査
      </td>
      <td>未調査</td><td>未調査</td><td>未調査</td>
      <td>
        <a href="https://arxiv.org/abs/2007.12099">論文</a><br>
        <a href="https://paperswithcode.com/paper/pp-yolo-an-effective-and-efficient">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>PSS</td><td>2021/01/28</td>
      <td>
        ・NMSフリーなモデルを学習するためにPSSヘッドを導入した論文<br>
        ・PSSヘッドにより1つの正解に対して1つだけ正例を出力するよう学習<br>
        ・YOLOXのNMSフリー用オプションとして使用
      </td>
      <td>1stage<br>(ancor free)</td><td>FCOS</td>
      <td>
        <a href="./cv_003_object_detection/pss.md">解説</a><br>
        <a href="https://arxiv.org/abs/2101.11782">論文</a><br>
        <a href="https://paperswithcode.com/paper/object-detection-made-simpler-by-eliminating">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>YOLOF</td><td>2021/03/17</td>
      <td>
        ・FPN構造の成功は複数解像度の特徴量ではなく分割統治法である<br>
        ・そのことから単一解像度の特徴量からのモデルを考案した<br>
        ・名前はYOLOが付いているが、ベースはRetinaNetの構造
      </td>
      <td>1stage<br>(ancor box)</td><td>ResNet-50<br>ResNet-101</td>
      <td>
        <a href="./cv_003_object_detection/yolo_f.md">解説</a><br>
        <a href="https://arxiv.org/abs/2103.09460">論文</a><br>
        <a href="https://paperswithcode.com/paper/you-only-look-one-level-feature">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>Swin Transformer</td><td>2021/03/25</td>
      <td>
        未調査
      </td>
      <td>未調査</td><td>未調査</td><td>未調査</td>
      <td>
        <a href="https://arxiv.org/abs/2103.14030">論文</a><br>
        <a href="https://paperswithcode.com/paper/swin-transformer-hierarchical-vision">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>OTA</td><td>2021/03/26</td>
      <td>
        ・ラベル割り当てを最適輸送問題(OTA)として解いたモデル<br>
        ・OTAはペアのロスを定義することに加え、需要量と供給量を定義<br>
        ・この問題設定かでSinkhorn-Knopp法で最適値を求める<br>
        ・YOLOXでこれの高速な手法が用いられている
      </td>
      <td>1stage<br>(ancor box)</td><td>ResNet-101<br>ResNeXt-101</td>
      <td>
        <a href="./cv_003_object_detection/ota.md">解説</a><br>
        <a href="https://arxiv.org/abs/2103.14259">論文</a><br>
        <a href="https://paperswithcode.com/paper/ota-optimal-transport-assignment-for-object">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>PP-YOLOv2</td><td>2021/04/21</td>
      <td>
        未調査
      </td>
      <td>未調査</td><td>未調査</td><td>未調査</td>
      <td>
        <a href="https://arxiv.org/abs/2104.10419">論文</a><br>
        <a href="https://paperswithcode.com/paper/pp-yolov2-a-practical-object-detector">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>YOLOX</td><td>2021/07/18</td>
      <td>
        ・YOLOに先端のラベル割り当て(OTA)やアンカーレス方式を導入した<br>
        ・OTAをより高速にするため近似計算するSimOTAを用いている<br>
        ・アンカーレスの方式はシンプルでYOLOv1に近い方式<br>
        ・YOLOv3をベースとしているため、特徴量融合もv3の方式となる<br>
        ・YOLOシリーズ特有のSingle HeadはDecoupledされている
      </td>
      <td>1stage<br>(ancor free)</td><td>Darknet-53<br>etc</td>
      <td>
        <a href="./cv_003_object_detection/yolo_x.md">解説</a><br>
        <a href="https://arxiv.org/abs/2107.08430">論文</a><br>
        <a href="https://paperswithcode.com/paper/yolox-exceeding-yolo-series-in-2021">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>YOLOP</td><td>2021/08/25</td>
      <td>
        未調査
      </td>
      <td>未調査</td><td>未調査</td><td>未調査</td>
      <td>
        <a href="https://arxiv.org/abs/2108.11250">論文</a><br>
        <a href="https://paperswithcode.com/paper/yolop-you-only-look-once-for-panoptic-driving">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>PP-PicoDet</td><td>2021/11/01</td>
      <td>
        未調査
      </td>
      <td>未調査</td><td>未調査</td><td>未調査</td>
      <td>
        <a href="https://arxiv.org/abs/2111.00902">論文</a><br>
        <a href="https://paperswithcode.com/paper/pp-picodet-a-better-real-time-object-detector">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>PP-YOLOE</td><td>2022/03/30</td>
      <td>
        未調査
      </td>
      <td>未調査</td><td>未調査</td><td>未調査</td>
      <td>
        <a href="https://arxiv.org/abs/2203.16250">論文</a><br>
        <a href="https://paperswithcode.com/paper/pp-yoloe-an-evolved-version-of-yolo">paperswithcode</a>
      </td>
    </tr>
    <tr>
      <td>ViTDet</td><td>2022/03/30</td>
      <td>
        未調査
      </td>
      <td>未調査</td><td>未調査</td><td>未調査</td>
      <td>
        <a href="https://arxiv.org/abs/2203.16527">論文</a><br>
        <a href="https://paperswithcode.com/paper/exploring-plain-vision-transformer-backbones">paperswithcode</a>
      </td>
    </tr>
  </tbody>
</table>

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

- ディープラーニングによる一般物体検出アルゴリズムまとめ </td><td> NegativeMindException
  - https://blog.negativemind.com/portfolio/deep-learning-generic-object-detection-algorithm/

- YOLOのv1～v5まで
  - https://qiita.com/tfukumori/items/519d84bf3feb8d246924

- YOLOシリーズについて2021年時点までの情報がいろいろ書いてある。
  - https://www.slideshare.net/ren4yu/you-only-look-onelevel-feature

- 2014～2019までの物体検知の歴史
  - https://github.com/hoya012/deep_learning_object_detection
