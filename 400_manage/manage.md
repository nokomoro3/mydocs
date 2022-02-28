# manage

## GitLabでの働き方

- https://learn.gitlab.com/c/gitlab-presentation-developers-summit?x=JBqxmQ
  - カメラはデフォルトオン
  - 1on1は1week / 25分推奨
  - 1on1 ≠ coffee chat
  - coffee chat以外は、アジェンダを準備する
  - 事前にアジェンダを教えた上で、同期参加の決定権は参加者にあり。
  - 同期ミーティングの前に、３回以上非同期を試みる（アジェンダのイメージがわく）
  - JDは、あるべき姿や責任が定義される
  - #thanksチャンネルがある

## 新人教育マニュアル

- https://discord.com/channels/906524238914138172/931386975565541396/937487648493273118


## スタンドアップミーティングのやり方

- スタンダップミーティングは役に立たない」
  - https://qiita.com/e99h2121/items/49742e20983c9ec971d4

## コーチング

- 問題を外から観察する人は、解決策にたどり着けない コーチングの第一人者が語る、ソリューションを作れる人の条件
  - https://logmi.jp/business/articles/325642

## 変更管理

- FRONTEO時代にまとめた変更管理メモ
  - 受け入れる体制があると思えなかったので、そっとお蔵入り。

```
・変更管理のキモ
　・トレーサビリティがあるか？

・トレーサビリティを担保するためにやるべきことを考える
　・今のあなたの作業は、担保されているかを常に意識

・開発側
　・ソースコード　（変更履歴をたどる）
　・設計書　（キモとなる部分、ソースコードで理解が困難な部分を補完）
　・インフラ
　　・製品としてのインフラはdockerでほぼ管理できる。
　　・加えて個別の手順書が必要な部分は作成が必要
　　　・クラウドとか、メール連携とか。
　　　・クラウドは、terraformとかいうツールもあるけど、使ったことはない。
　・保守管理
　　・何をいつアップデートする予定？した？
　・問題管理
　　・どういう問題が発生した？
　　・どういう対応をした？
　　・決してメンバーを攻めてはいけない

・研究側
　・解析サーバー
　　・同じくインフラ管理
　　・製品インフラよりは更新が頻繁かも。
　・ソースコード
　　・ある程度やるべき
　・設計書
　　・研究の場合はむしろここが大事
　　・アルゴリズムの設計は思想そのものなので、ソースコードで全て表現は困難
　　・設計仕様をノートブックに書くのはありかも
　　・そのノートブックは構成管理が必要か
```