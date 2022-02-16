# Beyond BatchNorm: Towards a Unified Understanding of Normalization in Deep Learning

## 論文について (掲載ジャーナルなど)
- [Lubana, E., Dick, R., & Tanaka, H. (2021). Beyond BatchNorm: Towards a Unified Understanding of  Normalization in Deep Learning. *Advances in Neural Information Processing Systems*, *34*.](https://proceedings.neurips.cc/paper/2021/file/2578eb9cdf020730f77793e8b58e165a-Paper.pdf)

## 概要
- Batach Normalization (BN) は成功を収めていて、多くの種類の正規化層が提案されてる
    - 活性度ベースの正規化層
    - グループノルムの正規化層

- 本論文では、正規化層の特性を調べた。知見は以下の通り。
    1.  BNのような活性度ベースの正規化層はResNetsでは◎だが、パラメトリックな手法では×
    2.  グループノルムを用いると、異なるサンプルに異なる活性度を割り当てられ、有益な順伝播性が確保できるが、レイヤノルムのモデルでは収束性が遅め
    3.  グループサイズが小さいと、初期の層の勾配ノルムが大きくなり、インスタンス正規化では学習が不安定になる（グループノルムでは、速度と安定性にトレードオフあり）


## 問題設定と解決したこと
- 

## 何をどう使ったのか
- 

## 主張の有効性の検証方法
- 

## 批評
- 参考

    - [@mosko_muleさんのツイート](https://twitter.com/mosko_mule/status/1493895026283192320)

        - >   GroupNormのグループ数が小さいとき（InstanceNorm）は活性値の多様性があるが勾配爆発が起きやすく（学習が不安定に）、グループ数が大きいとき（LayerNorm）は勾配爆発を防ぐものの活性値の多様性が減る（学習に時間がかかる）。BatchNormは前者に近そう。


## 次に読むべき論文
- 
