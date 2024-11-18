# Feature selection, $L_1$ vs. $L_2$ regularization, and rotational invariance

## 論文について (掲載ジャーナルなど)
- [Ng, A. Y. (2004, July). Feature selection, L 1 vs. L 2 regularization, and rotational invariance. In *Proceedings of the twenty-first international conference on Machine learning* (p. 78).](https://dl.acm.org/doi/pdf/10.1145/1015330.1015435?casa_token=DMNr6AxmPXsAAAAA:hTLsh38A9zlT4SL0Dot1bkCXQYe2SbsUjBdf5GQ1dwD0iXouy6Z7y-nrnuopmaRz8v4hGu8nGism4Gw)

## 概要
- 非常に多くの無関係な特徴が存在する場合の教師あり学習を考察
- 過学習を防ぐための2種類の正則化手法を研究
    - $L_1$正則化ロジスティック回帰は、標本複雑度（うまく学習するためのに必要なデータ数）は、irrelevantな特徴の数に対して対数的にしか増加しないことを示す
    - $L_2$正則化ロジスティック回帰、SVM、NNを含む**回転不変**なアルゴリズムでは、最悪の場合の標本複雑度が irrelevant な特徴の数に対して少なくとも線形に増加することを示す下界を与える


## 問題設定と解決したこと
- 問題設定:
    - 入力特徴量が多数存在し、かつターゲットをうまく近似するのに十分な特徴量の小さな部分集合が存在する場合の教師あり学習を考える
        - このケースの教師あり学習では、十分な学習データが無い限り、通常、過学習が問題となる
            - よくしられた事実として、学習誤差を最小化する正則化されていない識別モデルにおいて、標本複雑度はVC次元に対して線形に増加することが知られている
            - 多くのモデルでは、VC次元はパラメータ数に対してほぼ線形に成長し、パラメータ数は入力特徴量に対して少なくとも線形に増加することが一般的
            - 学習データセットのサイズが大きくない限り、過学習を防ぐために正則化などのテクニックが必要


## 何をどう使ったのか
- 

## 主張の有効性の検証方法
- 

## 批評
- 

## 次に読むべき論文
- 
