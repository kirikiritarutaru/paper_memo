# A ConvNet for the 2020s

## 論文について (掲載ジャーナルなど)
- [Liu, Z., Mao, H., Wu, C. Y., Feichtenhofer, C., Darrell, T., & Xie, S. (2022). A ConvNet for the 2020s. *arXiv preprint arXiv:2201.03545*.](https://arxiv.org/pdf/2201.03545.pdf)

## 概要
- CNNの設計空間を再検討し、純粋なCNNが達成できる性能の限界をテスト
- ResNetをViTの設計を参考に改良
  - その過程で、性能に寄与する要素を発見
  - Swin Transformerを超える性能を達成

## 問題設定と解決したこと
- Vision Transformer (ViT)がでてきて画像認識はさらに盛り上がっている
    - Swin Transformerが発表されて汎用的なビジョンバックボーンとして実用化されてる
        - ViTは入力サイズに対して$O(n^2)$のオーダー←高解像度の画像だとすぐに処理不能になるやで
        - スライディングウィンドウ戦略（ローカルウィンドウ内でのAttention）が導入される←これがSwin Transformer
            - ↑結局畳み込み的な構造っていうのが画像に関しては有効ってことやん
    
- CNN系のネットワークはViT系に駆逐されるほど弱いのか？
    - ViTの設計を真似してResNetを再構築してみたら、性能に寄与する要素を発見したぜ！

- この論文では、そのクールな要素を紹介するぜ！

## 何をどう使ったのか
- ResNetをDeiTやSwin Transformerと同様の学習手順で学習。そのときの性能をベースラインとする
    - 学習手順を変更するだけでも元のResNetよりも高精度を達成
        - 76.1%→78.8% (+2.7%)
            - **従来のConvNetsとViTの性能差のかなりの部分やんけ！**

    - 学習手順 
        - エポック数
            - (90→)300

        - 最適化手法
            - AdamW optimizer

        - データオーグメンテーション
            - Mixup
            - Cutmix
            - rand

        - 正則化スキーム
            - Stochastic Depth
            - Label Smoothing

- ベースラインのResNetに下記工夫を加える
    1.  マクロ設計
    2.  ResNeXt
    3.  逆ボトルネック
    4.  大きなカーネルサイズ
    5.  様々な層別ミクロ設計

- 加えた工夫の詳細
    - マクロ設計

        - ステージの計算比率の変更

            - 各ステージのブロック数の比率をSwin-Tと揃える
                - オリジナルのResNet-50：（3, 4, 6, 3）→変更後のResNet-50：（3, 3, 9, 3）

            - 78.8%→79.4% (+0.6%)

        - ステムを"Patchify"に変更

            - ネットワーク開始時の入力画像をダウンサンプリングする層に変更を追加
                - カーネルサイズ$7\times7$、ストライド2による畳込み層＋マックスプーリング（←これで$\frac{1}{4}$倍にダウン）から
                - カーネルサイズ$4\times4$、ストライド4によるの非重複畳み込みに変更

            - 79.4%→79.5% (+0.1%)

    - ResNeXt

- Swin Transformerに加えられた工夫をResNetに追加し、ConvNeXtを構築


## 主張の有効性の検証方法
- Swin Transformer に加えられた（Attention構造以外の）工夫をResNetにも追加
    - ResNetに工夫を加えて構築した **ConvNeXt** は Swin Transformer よりも高い性能を発揮
    - <img src="picture/ConvNets Figure2.png" alt="ConvNets Figure2" style="zoom:67%;" />
        - 論文図2より引用：付け加えられた工夫と加えた時に達成した精度（ImageNet Top 1 Acc）を示している


## 批評
- [機械学習チームで論文読み会を実施してみました（A ConvNet for the 2020s解説）](https://devblog.thebase.in/entry/2022/03/28/110000?utm_campaign=Weekly%20Kaggle%20News&utm_medium=email&utm_source=Revue%20newsletter)
    - ConvNetの論文についての要約

- やっぱり結論としてAttentionというアーキテクチャではなくて、学習手法の工夫のほうが精度向上に寄与しているという結果になったという報告
    - ShiftViTの論文でも同様の結論

- はたして、DNNモデルの性能は比較可能なのか？
    - 学習手法の工夫の組み合わせと独立に議論できるのか？？
- ImageNetのテストセットに対する過剰適合じゃないか？？
    - 精度確認→工夫追加→精度確認→…のプロセスだし
    - ConvNeXt だけがというよりもSwin-Tも含めて



## 次に読むべき論文
- Irwan Bello, William Fedus, Xianzhi Du, Ekin Dogus Cubuk, Aravind Srinivas, Tsung-Yi Lin, Jonathon Shlens, and Barret Zoph. Revisiting resnets: Improved training and scaling strategies. NeurIPS, 2021.
- Ross Wightman, Hugo Touvron, and Hervé Jégou. Resnet strikes back: An improved training procedure in timm. arXiv:2110.00476, 2021.
