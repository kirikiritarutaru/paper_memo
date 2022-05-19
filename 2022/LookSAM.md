# Towards Efficient and Scalable Sharpness-Aware Minimization

## 論文について (掲載ジャーナルなど)
- [Liu, Yong, et al. "Towards efficient and scalable sharpness-aware minimization." *arXiv preprint arXiv:2203.02714* (2022).](https://arxiv.org/pdf/2203.02714.pdf)

## 概要
- 損失関数の形状と汎化を結びつけるSAMがVision Transformersなどの大規模モデルの学習において大幅な性能向上を実証している
- SAMの課題として、SAMの更新則が各ステップで2回の逐次勾配計算を必要とし、計算オーバーヘッドが2倍になることである
- 本論文では、SAMの学習コストを大幅に削減する新規アルゴリズムLookSAMを提案する
    - LookSAMは、inner gradient の上昇のみを周期的に計算する機構


## 問題設定と解決したこと
- シャープな local minima はハマるとDNNの汎化性能を大きく低下させる
- local minima にハマる問題を緩和する方法が多く提案されている
- その中でも、特に有名なのが Sharpnes Aware Minimization (SAM)
- SAMによって**精度は**高くなるが、**学習に時間がかかる**問題がある
    - 更新ルールで各ステップ2回の勾配計算が必要になり、学習時間が2倍になる

- SAMの学習に時間がかかる問題を解決するLookSAMを提案
    - SAMの効率を向上させ、大規模な学習問題への適用を目指した

- LookSAMとは？
    - SAMの勾配の更新方向を、SGDの方向に平行な成分と、平坦な領域に向かって学習を偏らせる直交する成分の2つに分解
    - 各反復では、（近い場所では）勾配の方向が似ているので、勾配の方向を再利用する
    - 勾配計算の回数を減らしながら、SAMと同等の汎化性能を達成
- 論文の contributions
    1.  LookSAMの提案
    2.  LooK-LayerSAMの提案
        -   レイヤーワイズスケーリングルール（層ごとの重み付けのスケーリングルール）を採用した手法
        -   バッチサイズを**64k**に拡張でき、ViTを学習する際には従来の16倍速で学習できるようになった

    3.  LookLayerSAMによるViTの学習の高速化でSOTA
        -   バッチサイズ4kでViT-B-16の学習を0.7時間で終了
        -   ↑これ、ただただ計算機/GPUの性能がクソやばなのでは？？


## 何をどう使ったのか
- <img src="picture/LookSAM_Figure3.png" alt="LookSAM_Figure3" style="zoom:67%;" />

## 主張の有効性の検証方法
- 

## 批評
- 

## 次に読むべき論文
- 

