# Sharpness-Aware Minimization for Efficiently Improving Generalization

## 論文について (掲載ジャーナルなど)
- [Foret, P., Kleiner, A., Mobahi, H.,  & Neyshabur, B. (2020). Sharpness-aware minimization for efficiently improving generalization. *arXiv preprint arXiv:2010.01412*.](https://arxiv.org/pdf/2010.01412.pdf)

## 概要
- 大規模なモデルでは、学習損失の値によってモデルの汎化能力がほとんど保障されないという問題が存在
- 本論文では、損失関数の値とシャープネスを同時に最小化する最適化アルゴリズム **Sharpness-Aware Minimization (SAM)** を提案
    - 実装：[google-research/sam](https://github.com/google-research/sam)

- 様々なベンチマークで実験した結果、SAMを用いることによりモデルの汎化能力が改善されることを実証

## 問題設定と解決したこと
- DNNは、様々なタスクで既存の機械学習手法よりも高いパフォーマンスを達成しているが、成功の大部分は過剰なパラメータに依存している
- 過剰なパラメータ数は、学習データを容易に記憶することができ、簡単にオーバーフィットする問題も起こす
- 損失関数を学習データの集合上で単に最小化するだけでは、満足な汎化を達成することができない
- 損失関数の形状（ランドスケープ）は、
    - 一般的に複雑かつ非凸
    - ローカルおよびグローバルミニマムが多数存在
    - 異なるグローバルミニマムは異なる汎化能力を持つモデルをもたらす

- 本論文では、損失関数のランドスケープの幾何学と汎化能力の関連を直接利用した新しい最適化アルゴリズムSAMを提案する
    1.  損失関数の値とシャープネスを同時に最小化することにより、モデルの汎化能力を向上させることができる
    2.  広く研究されているコンピュータビジョンの課題およびモデルにおいて、モデルの汎化能力が向上することを確認
    3.  ラベルノイズに対して、頑健なモデルができることも確認
    4.  損失関数のシャープネスに対する新しい概念m-shapnessを考察することによって、損失関数のシャープネスと汎化能力の関係を詳しく考察

- 論文図1より引用
    - （左）SAMに切り替えることでエラーレートの減少幅。（右）SGDとSAMを用いた場合の損失関数の形状の比較
    - ![SAM_FIgure1](picture/SAM_FIgure1.png)

- SAMの結果から、Sharpnessと一般化について考察
    - m-sharpnes
    - hessian spectra


## 何をどう使ったのか
- 数式
    - (1)の上の式
        - 角括弧の項はパラメータ$w$から近傍のパラメータ値に移動することによって学習損失がどれだけ早く増加するかを測定する
        - これにより、$w$における$L_S$の Sharpness を捉える
        - Sharpness は学習損失値そのものと$w$の大きさに対する正則化をまとめたものである
            - Sharpnessが大きくなったら反則。ペナルティキッス。ということ

    - 勾配計算時、計算高速化のため**2次の項を削除する**
        - 2次の項を含めると**性能が低下**する
        - なぜ、性能が低下するのか？は今後の研究課題

- アルゴリズム概要
    1.  現地点の勾配を計算
    2.  坂を登り、登った先の勾配を計算
    3.  現地点から、「登った先の勾配」の方向に移動
- 論文図2より引用
    -   ![SAM_FIgure2](picture/SAM_FIgure2.png)

## 主張の有効性の検証方法
- コンピュータビジョン系のタスクでSAMの性能を確認
    - CIFAR-10, CIFAR-100, ImageNet

- SAMは2回勾配計算が必要なのでSAMでない最適化手法では、2倍のエポックを実行できるようにし、標準エポック数 or 2倍エポック数のどっちかでのベストスコアとSAMのスコアを比較
- 実験詳細
    - 画像分類

        - CIFAR-10, CIFAR-100で、様々なモデル（WRN, Shake-Shake, PyramidNet）を用いて、SAMとSGDを比較
            - 結果：SAMのテストセットエラーが常に低い

        - ImageNetで、ResNetを用いて、SAMとスタンダードな学習方法を比較
            - 結果：テストセットのTop-1 error rateでSAMのほうがベター。SAMでは、エポック数を200→400に伸ばしてもオーバーフィットしないことを確認。

    - ファインチューニング
        - EfficientNetを使って

    - ラベルノイズに対する頑健性

- 結論：
    - モデルの汎化性能を向上させることを確認


## 批評
- SAMと比較しているやつらって妥当なんか？
    - SGDとバトらせても…Adamとかは？

- 最適化手法を変更したらその他のパラメータでも妥当な値って変更されるのでは？

## 次に読むべき論文
- 

