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

## 主要なモデル一覧

<table>
  <thead>
    <tr>
     <th>名前</th>
     <th>発表年</th>
     <th>概要(3～5行)</th>
     <th>ベースCNN</th>
     <th>論文<br>実装例</th>
     <th>詳細</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>HOG + SVM</td>
      <td>2005</td>
      <td>
        <li>CNN誕生前のモデル</li>
        <li>HOGはセル内のpixel変化をとらえた特徴量</li>
        <li>それをSVMで多クラス分類する</li>
        <li>重複した検出はNMSで抑制する</li>
      </td>
      <td>CNN未使用</td>
      <td><a href="http://lear.inrialpes.fr/people/triggs/pubs/Dalal-cvpr05.pdf">論文</a></td>
      <td><a href="./cv_history_003_object_detection/hog_svm.md">詳細</a></td>
    </tr>
    <tr>
      <td>R-CNN</td>
      <td>2013.11</td>
      <td>
        <li>CNNを物体検知に適用した先駆け</li>
        <li>領域候補を古典的な手法(selective search)で領域候補を抽出</li>
        <li>領域候補をリサイズしてCNNに入力して特徴量を計算</li>
        <li>特徴量は後続の1クラスSVMとbounding boxの回帰モデルに入力する</li>
      </td>
      <td>
        AlexNet<br>
        VGG16
      </td>
      <td><a href="https://arxiv.org/abs/1311.2524">論文</a></td>
      <td><a href="./cv_history_003_object_detection/r_cnn.md">詳細</a></td>
    </tr>
    <tr>
      <td>SPP-net</td>
      <td>2014.06</td>
      <td>
        <li>R-CNNはリサイズが必要で画像が歪む</li>
        <li>また、領域候補(最大2000枚)すべてをCNNで処理する必要があって重い</li>
        <li>そこで入力画像での領域候補に該当する部分のCNNの特徴量マップを使う</li>
        <li>この特徴量マップをSPPで処理し、固定長の特徴量に変換</li>
        <li>その後は、R-CNNと同様</li>
      </td>
      <td>
        ZFNet<br>
        AlexNet<br>
        OverFeat
      </td>
      <td><a href="https://arxiv.org/abs/1406.4729">論文</a></td>
      <td><a href="./cv_history_003_object_detection/spp_net.md">詳細</a></td>
    </tr>
    <tr>
      <td>Fast R-CNN</td>
      <td>2015.04</td>
      <td>
        <li>Multi task lossによりクラス分類とbouding boxの位置推定を同時学習</li>
        <li>またSPP-netのSPPの代わりとしてRoI Poolingを使用して固定長に変換</li>
      </td>
      <td>
        VGG16
      </td>
      <td><a href="https://arxiv.org/abs/1504.08083">論文</a></td>
      <td><a href="./cv_history_003_object_detection/fast_r_cnn.md">詳細</a></td>
    </tr>
    <tr>
      <td>Faster R-CNN</td>
      <td>2015.06</td>
      <td>
        <li>領域候補計算用のRPN(Region proposal Network)を提案し全体をNNにした
        <li>これをEnd-to-Endモデルと呼ぶ)</li>
        <li>anchor box固定長(128x128)でアスペクト比を変更し9個準備</li>
        <li>anchor box毎に出力特徴量マップの各点で領域の有無とbounding boxの位置を推定</li>
        <li>RPNとFast-R CNNは別々に学習する必要がある</li>
      </td>
      <td>
        VGG16<br>
        ResNet101<br>
      </td>
      <td><a href="https://arxiv.org/abs/1506.01497">論文</a></td>
      <td><a href="./cv_history_003_object_detection/faster_r_cnn.md">詳細</a></td>
    </tr>
    <tr>
      <td>YOLO</td>
      <td>2015.06</td>
      <td>
        <li>領域推定とクラス分類を同時学習を実現した(You only look once)</li>
        <li>入力画像をグリッド分割(S x S)してそこにbounding boxがB個あると仮定</li>
        <li>各グリッドについてboxの信頼度(conf)と位置(h,w,x,y)、C個の各クラスの確率を計算</li>
        <li>上記を出力チャンネル数がS x S x ( B x 5 + C )のConvで実現</li>
        <li>1グリッドに対しB個しか検出できないず、速度優先のため性能はFaster R-CNNに劣る</li>
      </td>
      <td>
        独自実装
      </td>
      <td><a href="https://arxiv.org/abs/1506.02640">論文</a></td>
      <td><a href="./cv_history_003_object_detection/yolo.md">詳細</a></td>
    </tr>
    <tr>
      <td>SSD</td>
      <td>2015.12</td>
      <td>
        <li>YOLOと同じく領域推定とクラス分類を同時学習を実現した</li>
        <li>より多くの物体を様々なbounding boxで検出できるようになった</li>
        <li>上記のため、出力層だけでなく様々な解像度の特徴量マップを使用</li>
        <li>特徴量マップの解像度に応じてbounding boxがかわるように設計</li>
      </td>
      <td>
        VGG16
      </td>
      <td><a href="https://arxiv.org/abs/1512.02325">論文</a></td>
      <td><a href="./cv_history_003_object_detection/ssd.md">詳細</a></td>
    </tr>
    <tr>
      <td>R-FCN</td>
      <td>2016.05</td>
      <td>
        <li>Faster R-CNNをすべて畳み込み層にすることで高速化
        <li>RoI切り出し前にすべてposition sensitive mapに変換</li>
        <li>position sensitive map後にRoI切り出しすることで、後段のLinear層を削除</li>
      </td>
      <td>
        ResNet101
      </td>
      <td><a href="https://arxiv.org/abs/1605.06409">論文</a></td>
      <td><a href="./cv_history_003_object_detection/r_fcn.md">詳細</a></td>
    </tr>
    <tr>
      <td>FPN</td>
      <td>2016.12</td>
      <td>
        <li>ピラミッド構造により解像度と特徴量抽出を両立したモデル</li>
        <li>Faster R-CNNと同様にRPN(region proposal)と識別が別の2stage構成</li>
        <li>2stageのどちらともピラミッド構造化している</li>
      </td>
      <td>
        ResNet50
        ResNet101
      </td>
      <td><a href="https://arxiv.org/abs/1612.03144">論文</a></td>
      <td><a href="./cv_history_003_object_detection/fpn.md">詳細</a></td>
    </tr>
    <tr>
      <td>YOLOv2(YOLO9000)</td>
      <td>2016.12</td>
      <td></td>
      <td>独自実装(Darknet19)</td>
      <td><a href="https://arxiv.org/abs/1612.08242">論文</a></td>
      <td><a href="./cv_history_003_object_detection/yolo_v2.md">詳細</a></td>
    </tr>
    <tr>
      <td>RetinaNet</td>
      <td>2017.08</td>
      <td>
        <li>Focal lossを用いて1stageモデルの不均衡学習に対応</li>
        <li>ベースの構造は、FPNのピラミッド構造を用いている</li>
      </td>
      <td>ResNet50<br>ResNet101</td>
      <td><a href="https://arxiv.org/abs/1708.02002">論文</a></td>
      <td><a href="./cv_history_003_object_detection/retinanet.md">詳細</a></td>
    </tr>
    <tr>
      <td>Mask R-CNN</td>
      <td>2017.03</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1703.06870">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>RefineDet</td>
      <td>2017.11</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1711.06897">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>PANet</td>
      <td>2018.03</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1803.01534v4">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>YOLOv3</td>
      <td>2018.04</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1804.02767">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>M2Det</td>
      <td>2018.11</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1811.04533">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>CenterNet</td>
      <td>2019.04</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1904.07850">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>EfficientDet</td>
      <td>2019.11</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1911.09070">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>YOLOv4</td>
      <td>2020.04</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2004.10934">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>DETR</td>
      <td>2020.05</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2005.12872">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>YOLOv5</td>
      <td>2020.06</td>
      <td></td>
      <td></td>
      <td>未公開</td>
      <td></td>
    </tr>
    <tr>
      <td>YOLOF</td>
      <td>2021.03</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2103.09460">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>YOLOX</td>
      <td>2021.07</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2107.08430">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>YOLOP</td>
      <td>2021.08</td>
      <td></td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2108.11250">論文</a></td>
      <td></td>
    </tr>
  </tbody>
</table>

## 参考

- 概要がわかる
  - https://www.youtube.com/watch?v=5nmVHoA-A2E

- 物体検出についての歴史まとめ
  - https://qiita.com/mshinoda88/items/9770ee671ea27f2c81a9
  - https://qiita.com/mshinoda88/items/c7e0967923e3ed47fee5

- 物体検出モデルの進展
  - 物体検出モデルの進展 Part1 R-CNNからFaster R-CNNまで
    - https://qiita.com/TaigaHasegawa/items/b05110a2571a5289cbab
  - 物体検出モデルの進展 Part2 YOLOからR-FCNまで
    - https://qiita.com/TaigaHasegawa/items/a3cb98fb27cc7a9307b4
  - 物体検出モデルの進展 Part3 FPNとRetinaNet
    - https://qiita.com/TaigaHasegawa/items/653abc81ac4ee1f0d7b8

- Faster R-CNNはこれが一番分かりやすい。
  - https://medium.com/lsc-psd/faster-r-cnn%E3%81%AB%E3%81%8A%E3%81%91%E3%82%8Brpn%E3%81%AE%E4%B8%96%E7%95%8C%E4%B8%80%E5%88%86%E3%81%8B%E3%82%8A%E3%82%84%E3%81%99%E3%81%84%E8%A7%A3%E8%AA%AC-dfc0c293cb69

- SSDの解説は@xu1718191411さんのQiita記事がもっとも丁寧
  - 物体検出SSD詳しく紹介
    - https://qiita.com/xu1718191411/items/6ffdd2a65329b5b11704
    - https://qiita.com/xu1718191411/items/997190c6183114fcb343
    - https://qiita.com/xu1718191411/items/442e58f24e6608b6173c
    - https://qiita.com/xu1718191411/items/56002e07fe7618aef1d6
    - https://qiita.com/xu1718191411/items/cac5dd4e6d60038aaffb

- YOLOシリーズ徹底解説
  - https://deepsquare.jp/2020/09/yolo/
  - YOLOv1からv3まで。解説が分かりやすい。

- ディープラーニングによる一般物体検出アルゴリズムまとめ | NegativeMindException
  - https://blog.negativemind.com/portfolio/deep-learning-generic-object-detection-algorithm/

- YOLOのv1～v5まで
  - https://qiita.com/tfukumori/items/519d84bf3feb8d246924

- YOLO9000
  - https://qiita.com/miyamotok0105/items/1aa653512dd4657401db

- 2021年時点での情報がいろいろ書いてある。
  - https://www.slideshare.net/ren4yu/you-only-look-onelevel-feature

- DETR
  - https://club.informatix.co.jp/?p=1265
  - https://qiita.com/DeepTama/items/937e13f6beda79be17d8

- YOLOv3までの歴史と論文・実装リンク
  - https://github.com/hoya012/deep_learning_object_detection

- YOLOv1の良さげなnotebook
  - https://www.renom.jp/ja/notebooks/tutorial/image_processing/yolo/notebook.html
