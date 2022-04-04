# A ConvNet for the 2020s

## 論文について (掲載ジャーナルなど)
- [Liu, Z., Mao, H., Wu, C. Y., Feichtenhofer, C., Darrell, T., & Xie, S. (2022). A ConvNet for the 2020s. *arXiv preprint arXiv:2201.03545*.](https://arxiv.org/pdf/2201.03545.pdf)

## 概要
- CNNの設計空間を再検討し、純粋なCNNが達成できることの限界をテストした
- ResNetをViTの設計を参考に改良
  - その過程で、性能に寄与する要素を発見
  - Swin Transformerを超える性能を達成

## 問題設定と解決したこと
- Vision Transformer (ViT)がでてきて画像認識はさらに盛り上がっている
    - Swin Transformerが発表されて汎用的なビジョンバックボーンとして実用化されてる

- CNN系のネットワークはViT系に駆逐されるほど弱いのか？
    - ViTの設計を真似してResNetを再構築してみたら、性能に寄与する要素を発見したぜ！

- この論文では、そのクールな要素を紹介するぜ！

## 何をどう使ったのか
- 

## 主張の有効性の検証方法
- 

## 批評
- [機械学習チームで論文読み会を実施してみました（A ConvNet for the 2020s解説）](https://devblog.thebase.in/entry/2022/03/28/110000?utm_campaign=Weekly%20Kaggle%20News&utm_medium=email&utm_source=Revue%20newsletter)
    - ConvNetの論文についての要約

- やっぱり結論としてAttentionというアーキテクチャではなくて、学習手法の工夫のほうが精度向上に寄与しているという結果になったという報告
    - ShiftViTの論文でも同様の結論

- はたして、DNNモデルの性能は比較可能なのか？
    - 学習手法の工夫の組み合わせと独立に議論できるのか？？


## 次に読むべき論文
- 
