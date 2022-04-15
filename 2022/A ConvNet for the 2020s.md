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
- Swin Transformer に加えられた（Attention構造以外の）工夫をResNetにも追加
    - ResNetに工夫を加えて構築した **ConvNeXt** は Swin Transformer よりも高い性能を発揮
    - <img src="picture/ConvNets Figure2.png" alt="ConvNets Figure2" style="zoom:67%;" />
        - 論文図2より引用：付け加えられた工夫と加えた時に達成した精度（ImageNet Top 1 Acc）を示している
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
        - フィルタを異なるグループに分離する「グループ化畳み込み」を追加
            - バニラResNetよりもFLOPs/精度のトレードオフが良い
        - グループ化畳み込み→ネットワーク幅を拡張→深さ方向畳み込みで、グループの数がチャンネル数と等しくなるようにする
            - 深さ方向の畳み込みは、チャネル単位で動作するAttentionの重み付き和演算ににてる
                - ↑これは、空間次元の情報のみを混合する
            - 深さ方向の畳み込みと$1\times1$畳み込みの組み合わせは、空間とチャネルの混合の分離をもたらす
                - ↑ViTに共通する性質
        - 79.5%→80.5% (+1.0%)
    - 逆ボトルネック
        - すべてのTransformerブロックで重要な設計は、逆ボトルネックを作ること＝MLPブロックの隠れ次元が入力次元の4倍広くなること
            - ダウサンプリングブロックの$1\times1$畳み込み層のFLOPsが大幅に減少するので、ネットワーク全体のFLOPsは減少
            - 真ん中が逆ボトルネックのResNeXtブロック、右がConvNeXtの最終的なブロック<img src="picture/ConvNets Figure3.png" alt="ConvNets Figure3" style="zoom:67%;" />
        - 80.5%→80.6% (+0.1%)
            - この変更は、より大きいネットワーク（ResNet-200やSwin-B領域）だと改善幅が大きい（81.9%→82.6%）
    - 大きいカーネルサイズ
        - Swin Transformer のウィンドウサイズは$7\times7$とResNe(X)tのカーネルサイズ$3\times3$より大きい→参考にするやで
        - 深さ方向の畳み込み層の移動
            - 大きなカーネルを探索するために深さ方向の畳み込み層の位置を上げる
                - 図3の(b)→(c)
                    - 高速な$1\times1$の畳み込み層が重い処理を担当（チャネル数大）
                    - 大きいカーネルサイズを持つ畳み込み層が軽い処理を担当（チャネル数小）
            - 80.6%→79.9% (-0.7%)
                - これは、パラメータ数が減ったことによる効果（当然、FLOPsも減少）
        - カーネルサイズを大きくする
            - 様々なカーネルサイズで実験
                - サイズ：3, 5, 7, 9, 11
                - $7\times7$でカーネルサイズを大きくする効果が飽和することを確認
            - 79.9%→80.6% (+0.7%)
                - 精度復活
                    - なんで変更したん…計算量の減少のため？
    - 様々な層別ミクロ設計の変更
        - ReLU→GELU
            - ReLUのなめらかなバージョンGELU
            - 80.6%→80.6%
                - なんで採用するんの？
        - 活性化関数の数を減らす
            - 残差ブロックから$1\times1$層の1層を除いて、GELU層を削除し、Transformerブロックのスタイルを再現
            - 80.6%→81.3% (+0.7%)
                - すでにSwin-Tとほぼ同等の性能を達成
        - 正規化層の数を減らす
            - Transformerブロックは正規化層も少ない←真似るぜ
            - $1\times1$の畳み込み層の前に1つのBN層だけ残す
            - 81.3%→81.4% (+0.1%)
        - BN→LN
            - オリジナルのResNetのBNを直接LNに変えると精度低下
            - ネットワークアーキテクチャ・学習技術を変更してるので、BN→LNを再検討
                →ConvNeXtでは逆に精度微増
                - なんでやろね
            - 81.4%→81.5% (+0.1%)
        - ダウンサンプリング層の分離
            - Swin Transfoermersではステージ間に別のダウンサンプリング層が追加される←真似るぜ
            - 空間解像度をダウンサンプリングで減らすごとに正規化層を追加することで学習おｗ安定化
            - 81.5%→82.0% (+0.5%)
- Swin Transformerに加えられた工夫をResNetに追加し、ConvNeXtを構築


## 主張の有効性の検証方法
- ImageNetによりConvNeXtの精度の実験的評価

    - Swin-T/S/B/Lと同等のFLOPsをもつConvNeXt T/S/B/Lを構築し、精度を比較
        - バリエーションの違いは、チャンネル数と各ステージにおけるブロック数が異なるだけ

    - ConvNeXtのスケーラビリティを検証するため、より大規模なConvNeXt-XLを構築

- ImageNet-1K

    - 1000のオブジェクトクラスと120万枚の学習画像から構成されている
    - 検証データセットにおけるトップ1精度をレポートする
    - 学習セットアップは以下の通り<img src="picture/ConvNets Figure5.png" alt="ConvNets Figure5" style="zoom: 50%;" />
        - Exponential Mobing Average (EMA)がタイ規模なモデルの過剰適合を緩和することがわかったのでこれを使用

- 結果：
    - ConvNeXtは、同程度のFLOPsを持つSwin-Tよりも高いトップ1精度を達成

- ImageNet-22K

    - 大規模な事前学習を行った場合、ViTがつよつよという見解が広まっている
    - ConvNeXtでもImageNet-22Kで事前学習を行い、対よろする
    - 結果：
        - ConvNeXtが勝ち卍

- 物体認識とかセマンティックセグメンテーションのバックボーンにConvNeXtを使ってもSwin-Tよりよいことがわかったやで


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

- カーネルサイズの変更とReLU→GELUの変更精度変わらなんのになんで採用するんや…？

- TransformerとSwin Transformerの設計した人どうやってこの組み合わせ見つけれたん
    - 計算力の暴力か？

- Swin-Tとばっか比較してんな

    - Swin-Tの精度に貢献するポイントがAttentionだけじゃないと主張するためやけど、精度推しまくるならもっとほかのネットワークとも比較すべきでは
    - Conv系のネットワークと比較して、ConvNeXtがなぜ勝ったのか負けたのかの考察とか要りそう

- 4章のRemaks on model efficiency. の最後↓はえー

    >   vanilla ViTと比較すると、ConvNeXtとSwin Transformerはいずれも局所計算により、より有利な精度-FLOPsのトレードオフを示します。この効率の向上は、ConvNetの帰納的バイアスの結果であり、Vision Transformerの自己注意メカニズムとは直接関係ないことは注目に値します。

- Appendix F

    - ええこというてますやんか

        - >   我々は、アーキテクチャの選択は、シンプルさを追求しながらも、タスクの必要性を満たすべきであると考えている

    - ConvNeXtは画像認識・物体認識・セマンティックセグメンテーションではよかったけど、他のビジョンタスクだとTransormer系のほうがいいかもしれませんよという文脈ですね

        - ”画像認識”というタスク（のもつ特徴とか前提）に合わせてアーキテクチャを模索すべし



## 次に読むべき論文
- 学習テクニックと工夫するとResNet-50の性能を向上することができますよ論文
    - [Irwan Bello, William Fedus, Xianzhi Du, Ekin Dogus Cubuk, Aravind Srinivas, Tsung-Yi Lin, Jonathon Shlens, and Barret Zoph. Revisiting resnets: Improved training and scaling strategies. NeurIPS, 2021.](https://arxiv.org/pdf/2103.07579.pdf)
    - [Ross Wightman, Hugo Touvron, and Hervé Jégou. Resnet strikes back: An improved training procedure in timm. arXiv:2110.00476, 2021.](https://arxiv.org/pdf/2110.00476.pdf)
- Attention機構とConv系のネットワークを組み合わせよう論文
    - [Irwan Bello, Barret Zoph, Ashish Vaswani, Jonathon Shlens, and Quoc V Le. Attention augmented convolutional networks. In ICCV, 2019.](https://arxiv.org/pdf/1904.09925.pdf)
