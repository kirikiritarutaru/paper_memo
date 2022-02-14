# ArcFace: Additive Angular Margin Loss for Deep Face Recognition

## 論文について (掲載ジャーナルなど)
- [Deng, J., Guo, J., Xue, N., & Zafeiriou, S. (2019). Arcface: Additive angular margin loss for deep face recognition. In *Proceedings of the IEEE/CVF conference on computer vision and pattern recognition* (pp. 4690-4699).](https://arxiv.org/pdf/1801.07698.pdf)

## 概要
- 顔認識のための高い識別性を持つ損失関数Additive Angular Margin Loss (ArcFace, ArcMarginLoss)を提案
    - 顔認識以外にも使われるけど


## 問題設定と解決したこと
- 

## 何をどう使ったのか
- ArcFace: 
    $$
    L_3=-\frac{1}{N}\sum^N_{i=1}\log\frac{e^{s\left( \cos\left( \theta_{y_i}+m \right) \right)}}{e^{s\left( \cos\left( \theta_{y_i}+m \right) \right)}+\sum^n_{j=1, j\not=y_i}e^{s\cos\theta_j}}
    $$

    - ここで、$y_i$は$i$番目の正解ラベル、$n$はクラス数、$N$はバッチサイズ

- 画像から得られる特徴量$x_i$が対応するクラスのベクトルに一番近づくようにしたい

    - $\cos\theta_j$は特徴量がクラスの中心にどれだけ近いかを示すコサイン類似度
    - そこから少し角度がズレたとしても、大きい値になっていることを期待するためにマージン$m$が加えられる
    - マージンにより、クラス内のサンプルがクラスの中心に近くなるように促す
    - 適切なクラスから少し離れたとしてもそのクラスに分類されるように訓練が進むので、クラス間の識別性が上がる

## 主張の有効性の検証方法

- 

## 批評と備考
- 論文の図、直感的でめちゃめちゃわかりやすいな。こんなの真似して描いてみたい
- 参考記事
    - [【論文5分まとめ】ArcFace](https://zenn.dev/takoroy/articles/9766b03e3f832a)
    - 


## 次に読むべき論文
- 
