# YOLOX

- 題名: YOLOX: Exceeding YOLO Series in 2021
- 論文: [https://arxiv.org/abs/2107.08430](https://arxiv.org/abs/2107.08430)
- 公式実装
  - [https://github.com/Megvii-BaseDetection/YOLOX](https://github.com/Megvii-BaseDetection/YOLOX.)

## 概要

- YOLOのアンカーレス方式。
- その他の高度な技術として、decoupled headや先端のラベル割り当て戦略SimOTAを採用。
- CVPR2021のStreaming Perception Challengeの1位(YOLOX-L)を獲得した。

## 特徴

- YOLOはいまだ最高のトレードオフ性能だが、過去2年間にあった以下の進展がまだ取り入れられていない。
  - アンカーフリー
  - ラベル割り当て戦略
  - NMSなし(End-to-End)
- また、v4,v5はアンカーベース方式に過度に最適化されている可能性がある。
- そのためYOLOv3(YOLOv3-SPP)をベースとして選択。
- 実際、計算リソースが限られている場合などYOLOv3がまだよく使われている検出器でもある。

- 解像度640×640のCOCOにおいて、YOLOv3のAPを47.3%（YOLOX-DarkNet53）まで向上
  - 現在のYOLOv3の最善策（AP 44.3%, ultralytics version2 ）を大きく上回る。
- さらに、高度なCSPNetバックボーンと追加のPANヘッドを採用したYOLOX-LはYOLOv5-Lを上回る。
  - 640×640解像度のCOCOでAP50.0%を達成
  - 対応するYOLOv5-LをAP1.8%上回る
- また、サイズの小さいモデルでも設計戦略を検証
  - YOLOX-TinyとYOLOX-Nano（0.91Mパラメータと1.08G FLOPsのみ）は、
  - 対応するYOLOv4-TinyとNanoDet3に対してそれぞれ10%と1.8%のAP値で優れた性能を示しています。
- これらの成果は、ONNX、TensorRT、NCNN、Openvinoをサポートしたコードを公開している。

### baseline

- Darknet53のYOLOv3をベースラインとした。
- またいくつかの論文で言及されているYOLOv3-SPPのアーキテクチャを採用しています。
- またこれに対して以下を追加しています。
  - EMA weight update？
  - cosine lr schedule
  - IoU loss
  - IoU aware branch？
- clsとobjectnessの学習にはBCE lossを使用、bbox regressionの学習には、IoU lossを使用
  - これらはYOLOXの重要な改良点となるため、ベースラインにしている。
- またデータ拡張として以下を実施
  - RandomHorizontalFlip
  - ColorJitter？
  - multi-scale？
- RandomResiedCropはMosaicと重複するため、削除する。
- これらの工夫により、ベースラインはCOCOで38.5% mAPとなっています。

### Decoupled head

- YOLOシリーズ(v3～v5)は検出のhead部は、クラス確率とbbox回帰、objectnessがチャネル方向に結合された出力となっていた。

![](./img/yolo_x_coupled_head.png)

- 実験により、この結合された構造が性能を低下させる可能性があることがわかった。
- 非結合構造にすることで収束速度が大幅に改善する。

![](./img/yolo_x_training_curves.png)

- またEnd-to-End方式(NMSなし方式)では、Decouple構造の方が良い結果をもたらす。

![](./img/yolo_x_compare_couple_and_decouple.png)

- そこで、以下のようなDecouple構造を採用した。

![](./img/yolo_x_decouple_head.png)

### Strong data augmentation

- augmentationとして、mixupとmosaicを追加した。

- 具体的には、yolo_v4の詳細調査結果を参照。

- また、「close it for the last 15 epochs」とあるため最後の15epochsのみ実施していると思われる。

- データ補強を行う場合、ImageNetの事前学習はもう有益でないことがわかったので、ゼロから学習を行った。

### Anchor-free

- Anchorを使う場合、以下のような既知の問題がある。
  - 学習前にanchorをクラスタリングし、事前に決定する必要があるため、汎用性に乏しい。
  - アンカー数が膨大になるだけでなく、head部分の複雑さが増す要因となる。

- Anchor-freeとする方法は非常にシンプルであり、各グリッドごとに4つの値(左隅からのオフセットとH,W)を直接予測する。
  - 3から1に減らす(reduce the predictions for each location from 3 to 1)の意味は？
  - anchor box数が一つのポジションに一つになるってことかな

### Multi positives

- v3の割り当て規則と一致させるため、各オブジェクトに対して一つの正例（中心位置）のみを選択し、他は無視する。

- しかし、これらの他の点での予測を最適化することで、正例負例のアンバランスを緩和する可能性があります。

- Fcosの論文において、Center samplingと呼ばれる中央の3x3の領域を正例として割り当てる。

### SimOTA

- 以下に基づいた方式
  - https://arxiv.org/abs/2103.14259

- OTAの研究に基づき、高度なラベル割り当ての重要なポイントを整理した
  - 1). 損失・品質を考慮する
  - 2). Center Prior
  - 3). 各グラウンドトゥルースに対するポジティブアンカー4 の動的数（Dynamic Top-k と略す）
  - 4). グローバルビュー
 
- OTAは上記4つのルールを全て満たしているため、ラベル付与戦略の候補として選択した。

- 具体的には、OTAは以下のような内容である。
  - ラベル割り当てをグローバルな視点から分析
  - 割り当て手順をOptimal Transport (OT) 問題として定式化

- OTAを使用する際、OT問題をSinkhorn-Knoppアルゴリズムによって解くと、25%の学習時間が余分にかかる
  - 300エポックの学習には非常に高価であることがわかった。

- そこで、SimOTAと名付けた dynamic top-k strategy を用いて、近似解を求めるように単純化する。

- SimOTAの内容は以下の通りである。
  - まず、各予測-ground truthペアについて、コストや品質で表される pair-wise matching度合いを計算
  - 具体的に、SimOTAでは、ground truth: giと予測:pjの間のコストは、次のように計算される。

    - λはバランシング係数
    - L_cls_ij と L_reg_ij は gi と pj の間の classficiation loss と regression loss
- そして、giに対して固定された中心領域内で最もコストが低い上位 k 個の予測をその正サンプルとして選択
- 最後に、それらの正予測の対応するグリッドを正とし、残りのグリッドを負とする．
- なお、kの値はground-truthの違いにより異なる。
- 詳細については、OTAのDynamic k Estimation strategyを参照する

- SimOTAは学習時間を短縮するだけでなく、Sinkhorn-Knoppアルゴリズムにおけるsolverのhyperparameterを回避できる。

### OTAとは

- 以下の説明
  - https://arxiv.org/abs/2103.14259

- ラベルの割り当てを、最適輸送(Optimal Transport)問題としてとく。
  - DETRの最適割当問題と似ているが少し異なる。
  - ペア間のロスだけでなく、需要と供給のバランスが指標として存在する。

- 最小化するのは以下である。
  - c_ijはiとjの輸送コスト
  - π_ijは割り当てられた流量
  - d_jはjに必要な需要量
  - s_iはiが提供可能な供給量
  - iはsupplierの番号であり、jはdemanderの番号である。

![](./img/yolo_x_ota_formula.png)

- この問題は多項式時間で解くことが可能であるが、Sinkhorn-Knopp法で高速に説くことができる。

- ラベル割り当てでは、1画像内のm個のgtをsupplier、n個のbboxをdemanderとする。
- 各gtをk単位の正ラベルを持つとし、（つまり、s_i=k | i=1,2,...,m）
- 各bboxを1単位のラベルを必要とする（すなわち d_j=1 | j=1,2,...,n）として見ることにする。
- gt_iからbbox_jに1単位の正ラベルを輸送するコストc_ijは、それらのclsロスとregロスの加重和として定義する。

![](./img/yolo_x_ota_cost.png)

- ここでθはモデルのパラメータセットであり、P^cls_jは予測されるclassスコア、P^box_jはbbox回帰である。
- L_clsはCE_lossであり、L_regはIoU-lossである。
- L_regには、Focal-loss、GIoU、SmoothL1 Lossに置換が可能。
- αはバランス用の係数である。

- また最適輸送問題は通常、総供給量と総需要量が等しい必要がある。
- そのため、bgラベルを供給するsupplierも用意し、その供給量 n - m k として定義する。
- また輸送コストは正ラベルとは別に以下のように定義する。

![](./img/yolo_x_ota_cost_bg.png)

- L_clsはCE_lossとなる。

- 問題設定が完了したため、 off-the-shelf Sinkhorn-Knopp Iterationで最適輸送問題を解く。

- 最適化処理にはGPUで高速化可能な行列計算があるため、層トレーニング時間の増加は20%以下に収まり、推論時には全くコストは掛からない。

### Center Prior

- 各FPNから各gtと中心距離が近い(r x r)個のanchorを選択する。
- 選択に含まれないanchorには追加の定数コストを課す。

### Dynamic k Estimation

### End-to-End

- NMSを以下にのっとって取り除く。
  - https://arxiv.org/abs/2101.11782

- これにより、End-to-Endモデルが実現できるが、性能と推論速度がわずかに低下する。

- そのため、最終的なモデルには関与しないオプションモジュールとされている。

## 実験結果

- RetinaNetとの比較
  - RetinaNetはYOLOFと設定を合わせるため、GIoU, GN(Group Normalization), objectnessを導入したものを(+)記号として記載する。
  - YOLOFは、RetinaNetと同等性能で高い速度を達成している。

![](./img/yolo_f_experiment_vs_retinanet.png)

- DETRとの比較
  - DETRは大きい物体に強く、YOLOFは小さい物体に強い。
  - 推論速度はYOLOFが少し速く、学習時の収束は圧倒的にYOLOFが少ない。

![](./img/yolo_f_experiment_vs_detr.png)

- YOLOv4との比較
  - v4は様々なノウハウを組み合わせて調整されているため、ベースラインを目的としたYOLOFと厳密に比較が難しい。
  - ただし比較のため、ある程度カスタマイズしたYOLOF-DC5を作成した。
  - YOLOF-DC5は、13%高速に推論でき、総合性能もわずかに向上している。
  - 小さい物体はv4より劣り(-2.7 mAP)、大きい物体は良い結果となった(+7.1 mAP)。

![](./img/yolo_f_experiment_vs_yolo_v4.png)

## Ablation Experiments

### ResBlocksの数

- 4個を選択。増やす程よいがバランスをとった。

![](./img/yolo_f_ablation_resblocks.png)

### dilations setting

- 2,4,6,8を採用。

![](./img/yolo_f_ablation_dilations.png)

### shortcutの有無

- shortcutがない場合、性能が低下する。

![](./img/yolo_f_ablation_shortcut.png)

### Uniform Matching

- k=4で性能は頭打ちとなっている。

![](./img/yolo_f_ablation_uniform_matching_topk.png)

### other matchings

- ATSSは単一スケール特徴量を使う場合は、topk=15が最適であった。
- しかしそのATSSよりもUniform Matchingが優れている。
- またHungarian MatchingはUniform Matchingのtopk=1と同程度である。

![](./img/yolo_f_ablation_other_matchings.png)

### Number of Anchors

- RetinaNetでは、それぞれ322～5122の面積を持つ複数のレベル特徴（P3～P7）からアンカーが生成
- 各レベル特徴において以下のアンカーを敷き詰める。
  - サイズ｛2^0 , 2^(1/3) , 2^(2/3)｝
  - アスペクト比｛0.5, 1, 2｝
- 一方、YOLOFでは、アンカーを配置するのは1レベルの特徴量のみ。
- そこで、全ての物体のスケールをカバーするために以下とする。
  - 1つの特徴量マップに面積{322 , 642 , 1282 , 2562 , 5122}、サイズ{1}、縦横比{1}のアンカーを追加
  - 各位置に5個のアンカーを配置
- さらに、YOLOFにおいて、アンカーを多くした場合の影響を調査する。
  - RetinaNetにならって、以下を追加し、各位置に45個のアンカーを生成
    - 異なるサイズ（{2 0 , 2 1/3 , 2 2/3}）
    - 多くのアスペクト比（{0.5, 1, 2}）のした。
- 全ての結果を以下に示す。
  - アスペクト比を増やしてもYOLOFの性能は変わらない
  - サイズを増やすと性能が低下する

![](./img/yolo_f_ablation_anchor_box.png)

- したがって、YOLOFのアンカーはデフォルトで最小5個を追加することにした。

### YOLOF-DC5について

- YOLOFの性能を上げるために、C5特徴量よりも高分解能の特徴量マップ上で物体を検出する。
- DETRに従い、バックボーンの最終段にストライドを持たず、ダイレーションを持たせたバックボーンを構成。
- バックボーンの出力特徴量をDC5とし、ダウンサンプル率を16とする。
- 以下は、ResNet-50とResNet-101をバックボーンとしたCOCO val splitに対するYOLOF-DC5の結果である。
- YOLOF-DC5はオリジナルのYOLOFよりも高い性能を達成している。
- 一方、特徴の解像度がC5よりも大きいため、動作速度が低下していることがわかる。
- この結果を得るために、以下の変更をしている。
  - まず小さいアンカーを追加して1箇所あたり6アンカーとする
    - {16, 32, 64, 128, 256, 512}
  - 次にtopkを4から8に増やして正アンカーに対する無視閾値を0.15から0.1に変更する。
  - その他のパラメータは従来と同じである。

![](./img/yolo_f_ablation_yolof_dc5.png)

### エラーの分析

- 近年提案されたTIDEによる評価。
- TIDEはエラーを以下のように分類する。
  - Cls：分類エラー
  - Loc：定位エラー
  - Both：ClsとLocの両方エラー
  - Dupe：重複予測エラー
  - Bkg：背景エラー
  - Miss：欠損エラー
- 円グラフは各エラーの相対的な寄与度を、棒グラフはその絶対的な寄与度を示している。
- FPとFNはそれぞれfalse positiveとfalse negativeを意味する。

![](./img/yolo_f_ablation_error_analysis.png)

- 結果に対する分析
  - DETRはYOLOFよりもLoc Errorが大きく、これは回帰メカニズムに関係していると考えられる。
  - DETRはオブジェクトをアンカーフリーで回帰し、画像内のグローバルな位置を予測するため、定位が難しい。
  - 一方、YOLOFはあらかじめ定義されたアンカーに依存するため、DETRよりも高い欠損エラーがある。
  - YOLOFのアンカーはまばらであり、推論段階での柔軟性が十分でない。
  - 直感的には、ground truthの近くにあらかじめ定義されたanchorが存在しない状況がある。
  - したがって、YOLOFにアンカーフリー機構を導入することで、この問題を軽減できる可能性があり、今後の課題である。


## 参考

- 全編を読んだ後に気づいたけど、こちらに結構まとまっている。
  - https://www.slideshare.net/ren4yu/you-only-look-onelevel-feature/ren4yu/you-only-look-onelevel-feature
