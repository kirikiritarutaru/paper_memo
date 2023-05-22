# ConvNeXt V2: Co-designing and Scaling ConvNets with Masked Autoencoders

## 論文について (掲載ジャーナルなど)
- [Sanghyun Woo, Shoubhik Debnath, Ronghang Hu, Xinlei Chen, Zhuang Liu, In So Kweon, Saining Xie](https://arxiv.org/abs/2301.00808)

## 概要
- ConvNeXt に自己教師あり学習手法と新しいレイヤー、Global Response Normalization layer を組み合わせてみたよ
- 結果、ConvNeXtよりもベンチマークでの精度がよくなったよ
- 実装も公開しているよ
    - https://github.com/facebookresearch/ConvNeXt-V2


## 問題設定と解決したこと
- Transformer系の成功によって、自己教師あり手法が流行っている
- ConvNeXtアーキテクチャが純粋な畳み込みモデルでもスケーラブル（データを増やせば増やすほど性能UP）になることが示された

- 仮説：畳み込みモデルにも、Transformerで成功を収めている自己教師ありのエッセンスを入れたら良くなりそう
    - Vitでマスクドオートエンコーダ（MAE）がうまくいってるから、畳み込みモデルにも入れたろ！
- 結果：全然だめ！精度でない！
  - なぜ？
    - MAEがTransformerの時系列処理能力に特化した設計になってるから
      - Transformerでは、計算量の多いエンコーダーが visible patch に集中でき、事前学習コストを削減できる
    - 密なスライディングウィンドウを使用する畳み込みモデルと相性が悪い
  - 先行研究でも、マスクベースの自己教師あり学習による畳み込みモデルの訓練は困難であることが示されてる[43]

- じゃあ、**ConvNeXtモデルにマッチした、マスクベースの自己教師あり学習手法をMAEと同じ枠組みのもとで設計してみよう**
  - 工夫：
    - 疎な畳み込みでConvNeXtを実装
    - TransformerデコーダーをConvNeXtブロックに置き換え、アーキテクチャ全体を畳み込み系に統一して、事前学習の効率を向上
  - 結果：微妙！
    - 学習された特徴は有用だけど、Transformerベースのモデルより性能悪い

- まだまだ精度悪い原因を究明
  - 問題発見
    - ConvNeXtをマスクした入力で直接トレーニングした場合、MLP層で特徴が崩れる
  - 解決策
    - Global Response Normalization layer (GRN)
      - チャンネル間の特徴の競合を強化（←謎。本文をよく読もう）
  - GRNを追加することによってわかった懸念点
    - GRNを追加することが有効に働くケース＝モデルがマスクされたオートエンコーダで事前に訓練されたケース
      - ↑限定的！！
    - **教師あり学習のときと自己教師あり学習のときとで、アーキテクチャを変更した方がいいかもしれん**


## 何をどう使ったのか
- Global Response Normalization layer (GRN)


## 主張の有効性の検証方法
-


## 批評
-


## 次に読むべき論文
-
