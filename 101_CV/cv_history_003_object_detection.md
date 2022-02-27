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

- HOG + SVM @2005 (http://lear.inrialpes.fr/people/triggs/pubs/Dalal-cvpr05.pdf)
- R-CNN @2013.11 (https://arxiv.org/pdf/1311.2524.pdf)
- SPP-net @2014.06 (https://arxiv.org/pdf/1406.4729v4.pdf)
- Fast R-CNN @2015.04 (https://arxiv.org/pdf/1504.08083.pdf)
- Faster R-CNN @2015.06 (https://arxiv.org/pdf/1506.01497.pdf)
- YOLO @2015.06 (https://arxiv.org/pdf/1506.02640.pdf)
- SSD @2015.12 (https://arxiv.org/pdf/1512.02325.pdf)
- RetinaNet @2016.08 (https://arxiv.org/pdf/1708.02002.pdf)
- CenterNet @2019.04 (https://arxiv.org/pdf/1904.07850.pdf)
- DETR @2020.05 (https://arxiv.org/pdf/2005.12872.pdf)
  - https://club.informatix.co.jp/?p=1265
  - https://qiita.com/DeepTama/items/937e13f6beda79be17d8

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

## R-CNN @2013.11

- 原論文
  - https://arxiv.org/pdf/1311.2524.pdf

- 概要
  - CNNをobject detectionに適用する先駆けとなった論文。
  - 入力画像から領域候補(region proposal)を2000個まで切り出す。
  - それをリサイズ(224x224など)してCNNした特徴量で、classification(SVM)やbounding box推定を行う。

  ![](./img/cv_history_003_object_detection_r_cnn_architecture.png)


  ![](./img/cv_history_003_object_detection_r_cnn_flow.png)

  - 領域候補は、古典的なアルゴリズムを使ったselective searchで抽出する。

  - CNNには、AlexNet(T-Netと論文では呼ぶ)とVGG16(O-Netと論文では呼ぶ)を使っており、VGG16の方が高精度であった。
    - arxiv初版は、VGGより前なのでv5までの1年間の改版の過程で、大きく論文が変わっている。

  - selective searchは、Felzenszwalb法でセグメンテーションした結果(region)をもとに、
  regionの色、テクスチャ、サイズ、オーバーラップしている領域の４つで類似度を計算してマージしていくことで領域候補(region proposal)を計算する。
  (テクスチャは模様のことで、LBPを用いて計算する。)
    - 詳細は以下に解説あり。
      - https://blog.shikoan.com/selective-search-rcnn/
    - Felzenszwalb法は以下を参照。
      - https://irohalog.hatenablog.com/entry/2014/10/05/213948
      ![](./img/cv_history_003_object_detection_felzenszwalb_segmentation.png)

    - LBPについては以下を参照。
      - https://qiita.com/tancoro/items/959ae9c14048c06bea8e

- 手順
  - CNN(AlexNetやVGG16など)をILSVRC2012のデータセットでpre-trainingする。
  - Domain specificなfine-tuningを実施。データはselective searchで得たregion proposalを使う。
    - 最終層のみ物体検出したいクラス数N+1の層に変更して学習。
    - region proposalで得られた領域と正解領域とのIoUが0.5以上のところをpositive, それ以下をnegativeとして学習。
  - CNNで得た特徴量を元に、N個の1-class linear SVMを学習する。その際、正解領域とのIoUが0.3以下は負例として学習する。
    - CNNとSVMsで正解のIoU閾値が異なるが、現在の調整値が最も良い結果だったようである。
    - CNN単体での学習も考えたようだが、性能が低下したとのこと。調整をすれば性能が近づきシンプルになる可能性も言及している。(Appendix B.)
  - またlocalizationの改善のため、bounding-boxの推定を、CNNで得た特徴量を元に行う。
    - x,y,w,hの線形回帰問題(リッジ回帰)として学習する。
    - モデルとしては、region proposalからground truthまでの変換式を準備し、その変換式の係数を学習する。(Appendix C.)
    - SVMと同様、N個のクラス毎に学習する。その際、正解領域とのIoUが0.6以上のregion proposalの中で
    最もIoUが大きいもののみを学習データとする。
    - このbounding-box推定とSVMs学習を繰り返すループも実現できるが、改善しなかったとのこと。
  - 重複領域は、NMSで削除する。
    - NMSのIoU閾値は0.3である。

## SPP-net @2014.06

- 原論文
  - https://arxiv.org/pdf/1406.4729v4.pdf

- 概要
  - 入力画像に対する1回のCNNで画像内のすべてのregion proposalを処理できるようにした物体認識モデル。
  - R-CNNは最大で2000枚の画像をCNNで処理する必要があったが、これを1枚で可能とした。
  - 入力画像のregion proposalに対応する、出力層の特徴量マップを使い、これをSPP(spatial pyramid pooling)で固定長の特徴量に変換する。
  - その後は、R-CNNと同様にone-class SVMとbownding boxの回帰モデルで、処理をする。

  ![](./img/cv_history_003_object_detection_sppnet_architecture.png)

  - SPP(spatial pyramid pooling)部分の詳細は下記。

  ![](./img/cv_history_003_object_detection_spatial_pyramid_pooling.png)
  
  - CNN(AlexNet等、いくつか種類あり)のpre-trainingをR-CNNと同様に行うが、パラメータを更新するのは、SPP以降のみとした。
    - Fast R-CNNの論文では、CNNが多層ではない場合はこれで十分だが、多層の場合はすべてのパラメータをfine-tuningした方がよいと言及している。
  - R-CNNと異なり画像のリサイズ等は不要で処理が可能。
  - region proposalの領域は、出力層でつぶれない程度に大きさに下限が設けられている。

- 手順
  - 原論文参照。

## Fast R-CNN @2015.04

- 原論文
  - https://arxiv.org/pdf/1504.08083.pdf

- 概要
  - SPP-netのSPPの代わりにRoI PoolingというPoolingを使用。
  - loss計算にMulti-task Lossを使用し、classificationとbounding box regressorを同時に学習。

  ![](./img/cv_history_003_object_detection_fast_r_cnn_architecture.png)

  - RoI PoolingはSPPよりもシンプルであり、RoI領域(出力層におけるregion proposalの領域)をグリッド分割し、各グリッド毎にmax poolingを実施する方式である。
  - グリッドサイズはH, WのHyper parameterで、前段のCNNのアーキテクチャにより変わる。
    - VGG16の場合、linear直前のmax poolingが7x7なので、H=W=7となる。
  - CNNをFast R-CNN向けに変換する場合、以下のステップを経る。
    - 最後のmax pooling処理を、RoI poolingに置き換える。
    - その後、いくつかのlinear層を得て、classification用のlinear+softmaxとbounding box用のlinear層に分岐させる。
  - Multi-tasl lossは、cross entropy + lamda * regression errorで計算する。
    - ここで、regression errorは、backgroundの場合は使わない。
    - 回帰モデルに使う損失関数もR-CNNのときから変わっている。
      - L2損失からL1損失となり、外れ値に強いロス関数となっている。
    - lambdaは1として実験されている。
  - 高速化の工夫として、mini-batchの作成方法にも階層的サンプリングという工夫をしている。
    - R-CNNやSPPは、ROIレベルでサンプリングを実施した。つまりbatch size=128の場合、128個の入力画像を使う。
    - Fast R-CNNは、まず入力画像をN個サンプリングし、N個の画像からbatch size/N個のROIをサンプリングすることで、mini-batchを作る。
    - Nを小さくすることで、CNN部のfeedfowardする回数が少なくて済む。
    - N=2、batch size=128で良好な結果を得ることができている。
  - その他、サンプリングはmini-batch内の25%を正例とし、IoUが0.5以上を正例、IoUが0.1以上0.5未満を負例とする。
  - 更にlinear層の高速化手段としてSVDでの低ランク行列への近似を行う。

  ![](./img/cv_history_003_object_detection_fast_r_cnn_svd.png)

## Faster R-CNN @2015.06

- 原論文
  - https://arxiv.org/pdf/1506.01497.pdf

- 概要
  - region proposalをCNNで実現し、全体で初めてEnd-to-Endの物体検出を実現。
  - region proposal用のCNNはRPN(Region Proposal Networks)と呼び以下のような形式となる。
    - 入力は入力画像でベースとなるCNNで特徴量を抽出する。
      - ベースモデルはZFNet(AlexNetの改良版)、VGGなどを使う。
    - 出力は以下の2系統となる。
      - (anchor box数k x 2(0,1判定) ,h ,w)
      - (anchor box数k x 4(ground truthからのx,y,h,wの誤差) ,h ,w)
    - anchor boxの中心位置は、特徴量マップの各pixelとなるため、特徴量マップのサイズ分(h,w)の出力を持つ。
    - 上記の出力系統を実現するために以下をベースモデルに対して追加する。
      - conv3x3のConv層(channel数はベースの最終層と同じでVGGならば512)
        - その際最後のlinear層手前のmax poolingは削除する。
      - その後以下の二系統のconv1x1のConv層に分岐する。
        - チャンネル数がk x 2のConv
        - チャンネル数がk x 4のConv
    - anchor boxの数は以下の9個が使われる。
      - アスペクト比1:1の場合に3つのサイズ 128x128, 256x256, 512x512(画素単位は元画像レベル)
      - 上記に対して、pixel数を一定としたままで、アスペクト比を1:2, 2:1でそれぞれ作成
    - anchor boxが元画像からはみ出す場合は削除される。
    - RPN学習時は、ground truthとのIoUに応じて、IoUが0.3以下を負例、0.7以上を正例として学習します(間は中途半端として学習しない)。
  



## 参考

- 概要がわかる
  - https://www.youtube.com/watch?v=5nmVHoA-A2E

- 物体検出についての歴史まとめ
  - https://qiita.com/mshinoda88/items/9770ee671ea27f2c81a9
  - https://qiita.com/mshinoda88/items/c7e0967923e3ed47fee5

- Faster R-CNNはこれが一番分かりやすい。
  - https://medium.com/lsc-psd/faster-r-cnn%E3%81%AB%E3%81%8A%E3%81%91%E3%82%8Brpn%E3%81%AE%E4%B8%96%E7%95%8C%E4%B8%80%E5%88%86%E3%81%8B%E3%82%8A%E3%82%84%E3%81%99%E3%81%84%E8%A7%A3%E8%AA%AC-dfc0c293cb69
  
- YOLOのv1～v5まで
  - https://qiita.com/tfukumori/items/519d84bf3feb8d246924

- YOLO9000
  - https://qiita.com/miyamotok0105/items/1aa653512dd4657401db

- SSD
  - https://blog.negativemind.com/2019/02/26/general-object-recognition-single-shot-multibox-detector/

- YOLOシリーズ徹底解説
  - https://deepsquare.jp/2020/09/yolo/

- ディープラーニングによる一般物体検出アルゴリズムまとめ | NegativeMindException
  - https://blog.negativemind.com/portfolio/deep-learning-generic-object-detection-algorithm/
