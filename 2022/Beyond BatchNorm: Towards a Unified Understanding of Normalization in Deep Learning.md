# Beyond BatchNorm Towards a Unified Understanding of Normalization in Deep Learning

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
- DNNを効果的に学習したい場合、正規化手法を用いると下記の効果がある
    1.  より深い層に有益な活性化パターンを伝播する
    2.  重みの初期値への依存度の低減
    3.  異常な固有値の除去による収束の高速化
    4.  （適応的最適化手法と同等の効果を持つ）学習率のオートチューニング←論文要参照
    5.  Loss landscape のスムージング

- しかし、アプリケーションやシナリオによっては、正規化手法は効果が小さかったり邪魔になるケースもある
    1.  BNはバッチサイズが小さいとき、学習に苦労する（＝学習が遅い or 安定しない）
    2.  学習時とテスト時の分布がシフトする環境では、BNはモデルの精度を損なう
    3.  メタ学習では、transductive inference につながる←？
    4.  敵対学習では、間違った分布を推定してクリーンサンプルと敵対サンプル両方の精度を損なう

- 上記の欠点に対処するため、最近（2021年）では、代替となる正規化レイヤー（以降、正規化器・ノーマライザーと呼称）を提案している
    - Scaled Weight Standardization
        - BNを置き換え
        - ResNetの前方伝播の挙動を一致させる←どゆこと？

    - GroupNorm
        - レイヤー内の複数のチャンネルをグループ化して正規化
        - バッチに依存しない手法

    - 正規化と活性化層の両方を探索するための進化的アルゴリズム

- これらの研究では、学習のシナリオによってはBNと同等以上の効果があることが主張されている
- 疑問：**これらの手法はBNを置き換えられているのか**
- 調べた結果、**BNを含む全ての正規化手法に、明らかに成功/失敗のモードが存在する**という知見を得た
    - 安定した順伝搬
        - BNと同様、活性化ベースのノーマライザーがResNetsにおける勾配爆発を防ぐ
        - Weight Normalization のようなパラメトリックなノーマライザーはこの性質を持たない

    - 情報的な順伝搬（＝活性値の多様性がある、ということ）
        - 異なる入力に対し、異なる活性値を生成するノーマライザーが学習速度に大きく影響する
            - わかりにくいな…すなわち！
                - 活性値の多様性があるノーマライザーは、勾配の向きがいろんな方向を向いているので学習が早くなる傾向にある
                - 実際、実験すると活性値のコサイン類似度が低いほど、学習が早い・コサイン類似度が高いほど、安定性がある傾向になる
                - 層をかさねるにつれてコサイン類似度が高くなる傾向あり

        - グループサイズが小さい場合＝グループの数が多い場合、GroupNormを使用すると、異なる入力に対して活性値が多様化
            - Instance Normalization はグループサイズが1に等しいGroupNormなので、 学習早め
            - LayerNorm はグループサイズがレイヤ幅に等しいGroupNormなので、学習遅め

    - 安定な逆伝搬
        - 個々のサンプルやチャネルの統計に依存する正規化手法（たとえば、Instance Normalization）は勾配爆発し、学習が不安定化しやすい
        - GroupNormでチャンネルをグループ化することで緩和される
        - グループサイズによって学習速度と学習の安定性のトレードオフになる
            - グループサイズ小＝学習速度↑・安定性↓
            - グループサイズ大＝学習速度↓・安定性↑


## 何をどう使ったのか
-   10個の正規化レイヤーについて調べた
    -   活性化ベースのレイヤー
        1.  BatchNrom (BN)
        2.  LayerNrom (LN)
        3.  Instance Normaliztion (IN)
        4.  GroupNorm (GN)
        5.  Filter Response Normalization (FRN)
        6.  Variance Normalization (VN)
        7.  EvoNormBO
        8.  EvoNoRIMSO
    -   パラメトリックなレイヤー
        1.  Weight Normalization (WN)
        2.  Scaled Weight Standardization (SWS)

## 主張の有効性の検証方法
- ノーマライザー、ネットワーク、アーキテクチャ、バッチサイズ、学習率を変えて実際にやってみた
    - CIFAR-100で調べた場合
        - LayerNorm：比較的遅い速度で収束することが多い
        - Weight Normalization：ResNetsでは全く学習できない
        - Instance Normalization：residual では**ない**ネットワークでは特に小さいバッチサイズで不安定な学習になる

## 批評
- 正規化手法のpros, consのまとめめっっっちゃ勉強になるな…

- 参考

    - [@mosko_muleさんのツイート](https://twitter.com/mosko_mule/status/1493895026283192320)

        - >   GroupNormのグループ数が小さいとき（InstanceNorm）は活性値の多様性があるが勾配爆発が起きやすく（学習が不安定に）、グループ数が大きいとき（LayerNorm）は勾配爆発を防ぐものの活性値の多様性が減る（学習に時間がかかる）。BatchNormは前者に近そう。


## 次に読むべき論文
- 
