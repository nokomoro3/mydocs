# classification

## 主要なモデル一覧

<table>
  <thead>
    <tr>
     <th>名前</th>
     <th>発表年</th>
     <th>概要(3～5行)</th>
     <th>論文<br>実装例</th>
     <th>詳細</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>LeNet</td>
      <td>1989</td>
      <td></td>
      <td><a href="https://direct.mit.edu/neco/article-abstract/1/4/541/5515/Backpropagation-Applied-to-Handwritten-Zip-Code?redirectedFrom=fulltext">論文</a></td>
      <td><a href="./cv_002_classification/lenet.md">詳細</a></td>
    </tr>
    <tr>
      <td>AlexNet</td>
      <td>2012.09</td>
      <td></td>
      <td><a href="https://dl.acm.org/doi/abs/10.1145/3065386">論文</a></td>
      <td><a href="./cv_002_classification/alexnet.md">詳細</a></td>
    </tr>
    <tr>
      <td>VGGNet</td>
      <td>2014.09</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1409.1556">論文</a></td>
      <td><a href="./cv_002_classification/vgg.md">詳細</a></td>
    </tr>
    <tr>
      <td>GoogLeNet(Inception v1)</td>
      <td>2014.09</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1409.4842">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>ResNet</td>
      <td>2015.12</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1512.03385">論文</a></td>
      <td><a href="./cv_002_classification/resnet.md">詳細</a></td>
    </tr>
    <tr>
      <td>Inception-v4</td>
      <td>2016.02</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1602.07261">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>SqueezeNet</td>
      <td>2016.02</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1602.07360">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>DenseNet</td>
      <td>2016/08/25</td>
      <td>
        <li>ResNetを進化させ、より効率的に深くまで情報を伝搬させるdense blockを構成。</li>
        <li>dense blockはそのレイヤまでのblock内すべての出力を入力として用いる。</li>
        <li>また連結方法は加算ではなく、concatenateすることにより情報が消えることを防ぐ。</li>
        <li>ResNetよりモデル規模を縮小し、性能も改善。</li>
      </td>
      <td><a href="https://arxiv.org/abs/1608.06993">論文</a></td>
      <td><a href="./cv_002_classification/densenet.md">詳細</a></td>
    </tr>
    <tr>
      <td>Xception</td>
      <td>2016.10</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1610.02357">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>ResNeXt</td>
      <td>2016.11</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1611.05431v2">論文</a></td>
      <td><a href="./cv_002_classification/resnext.md">詳細</a></td>
    </tr>
    <tr>
      <td>MobileNet</td>
      <td>2017.04</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1704.04861">論文</a></td>
      <td><a href="./cv_002_classification/mobilenet.md">詳細</a></td>
    </tr>
    <tr>
      <td>ShuffleNet</td>
      <td>2017.07</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1707.01083">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>SENet</td>
      <td>2017.09</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1709.01507">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>MobileNetV2</td>
      <td>2018.01</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1801.04381">論文</a></td>
      <td><a href="./cv_002_classification/mobilenet_v2.md">詳細</a></td>
    </tr>
    <tr>
      <td>Big-Little Network</td>
      <td>2018.07</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1807.03848">論文</a></td>
      <td><a href="./cv_002_classification/big_little.md">詳細</a></td>
    </tr>
    <tr>
      <td>ShuffleNetV2</td>
      <td>2018.07</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1807.11164">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>Mnasnet</td>
      <td>2018.07</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1807.11626">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>Octave Convolution</td>
      <td>2019.04</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1904.05049">論文</a></td>
      <td><a href="./cv_002_classification/octave.md">詳細</a></td>
    </tr>
    <tr>
      <td>MobileNetV3</td>
      <td>2019.05</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1905.02244">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>EfficientNet</td>
      <td>2019.05</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1905.11946">論文</a></td>
      <td><a href="./cv_002_classification/efficientnet.md">詳細</a></td>
    </tr>
    <tr>
      <td>MixNet</td>
      <td>2019.07</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1907.09595">論文</a></td>
      <td><a href="./cv_002_classification/mixnet.md">詳細</a></td>
    </tr>
    <tr>
      <td>Noisy Student</td>
      <td>2019/11/11</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/1911.04252">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>CSPNet</td>
      <td>2019/11/27</td>
      <td>
        <li>DenseNetの更なる高速化。安価なGPUで動作することを念頭に改良。</li>
        <li>ベースレイヤを2つに分割し、2つのパスで伝搬する情報を効率化している。</li>
        <li>これは他のネットワークにも容易に適用可能で、計算量を10～20%削減し、さらに精度を向上。</li>
      </td>
      <td><a href="https://arxiv.org/abs/1911.11929">論文</a></td>
      <td><a href="./cv_002_classification/cspnet.md">詳細</a></td>
    </tr>
    <tr>
      <td>RegNet</td>
      <td>2020.03</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2003.13678">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>Vision Transformer</td>
      <td>2020.09</td>
      <td></td>
      <td><a href="https://openreview.net/forum?id=YicbFdNTTy">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>DeiT</td>
      <td>2020.12</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2012.12877">論文</a></td>
      <td></td>
    </tr>
    <tr>
      <td>EfficientNetV2</td>
      <td>2021.04</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2104.00298">論文</a></td>
      <td></td>
    </tr>
    <tr>
    <tr>
      <td>ConvNeXt</td>
      <td>2022.01</td>
      <td></td>
      <td><a href="https://arxiv.org/abs/2201.03545">論文</a></td>
      <td></td>
    </tr>
  </tbody>
</table>


## 参考

- [2019.10.30] 2019年最強の画像認識モデルEfficientNet解説
  - https://qiita.com/omiita/items/83643f78baabfa210ab1

- [2020.09.09] MobileNet(v1,v2,v3)を簡単に解説してみた
  - https://qiita.com/omiita/items/77dadd5a7b16a104df83

- [2021.04.17] EfficientNet B0〜B7で画像分類器を転移学習してみる
  - https://zenn.dev/kleamp1e/articles/202104-efficientnet

- [2021.07.30] EfficientNet: 複合スケールによる効率的な画像分類器
  - https://kikaben.com/efficientnet/

- Neural Network Console
  - https://www.youtube.com/channel/UCRTV5p4JsXV3YTdYpTJECRA

- 6つのモデルでのSwish関数の実験
  - https://ichi.pro/6-tsu-no-moderu-de-no-swish-kansu-no-jikken-265570078399001

- [2019.10.14] 【深層学習】CNNを用いた画像分類手法まとめ（VGG, ResNet, Inceptionなど）
  - https://ys0510.hatenablog.com/entry/cnn_backbone

- ResNetおよびDenseNetの解説
  - https://deepsquare.jp/2020/04/resnet-densenet/

- ResNetからResNextまで
  - https://cvml-expertguide.net/terms/dl/cnn-backbone/resnet/

- 古めな記事だが、ResNetの詳細が記載
  - https://deepage.net/deep_learning/2016/11/30/resnet.html

- 各種モデルサイズの比較表がある。
  - https://keras.io/ja/applications/

- [2021.05.03] 画像認識の大革命。AI界で話題爆発中の「Vision Transformer」を解説！
  - https://qiita.com/omiita/items/0049ade809c4817670d7

- [2020.03.07] 画像認識の最新SoTAモデル「Noisy Student」を徹底解説！
  - https://ai-scholar.tech/articles/treatise/noisy-student-ai-379

- [2021.04.13] 2021年最強になるか！？最新の画像認識モデルEfficientNetV2を解説
  - https://qiita.com/omiita/items/1d96eae2b15e49235110

- [2022.01.13] Transformer(ViT)系より良いConvだけのネットワーク出たよ（画像認識向け）
  - https://qiita.com/TeamN/items/edee1b3803a1d77fc252

- B0～B7の構造
  - https://towardsdatascience.com/complete-architectural-details-of-all-efficientnet-models-5fd5b736142