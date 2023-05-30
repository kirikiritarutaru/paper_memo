# Transformers in Time Series: A Survey
## 論文について (掲載ジャーナルなど)
- [Qingsong Wen, Tian Zhou, Chaoli Zhang, Weiqi Chen, Ziqing Ma, Junchi Yan, Liang Sun](https://arxiv.org/abs/2202.07125)


## 概要
- Transformerは、自然言語処理・コンピュータビジョンの多くのタスクで高性能
  - もちろん、時系列データの処理に対しても注目されている
- Transformerの長所である「長距離の依存関係や相互作用を捉える能力」は時系列モデリングにおいて特に有利
- 本論文では、時系列モデリングのためのTransformerスキームを系統的にレビューした
  - ネットワーク構造の観点とアプリケーションの観点からレビュー
    - ネットワーク構造：時系列解析の課題に対応するためにTransformerに追加された修正点をまとめる
    - アプリケーション：予測、異常検知、分類など一般的なタスクに基づき時系列Transformerを分類
    - <img src="picture/Transformers in Time Series Figure 1.png" alt="Transformers in Time Series Figure 1" style="zoom:50%;" />

## 問題設定と解決したこと
- Transformer の利点
  - 逐次的なデータにおける長距離の依存間けいや相互作用に対して優れたモデリング能力を持つ
- Transoformer 上記の特徴は**時系列モデリング**に適している
- 本論文では、時系列のTransformerに関する体系的かつ包括的な調査を行い、これを報告する
- 時系列Transformerの下記2点に着目した新しい分類法を提案
  - ネットワーク修正
    - 時系列モデリングの性能を最適化することを目的として、モジュールとアーキテクチャの両方で行われた改善についてまとめ
  - アプリケーションドメイン
    - 予測、異常検知、分類を含む時系列タスクのTransformerを分類しまとめ
      - 各時系列Transformerの強みと限界について分析

## 何をどう使ったのか
-

## 主張の有効性の検証方法
-

## 批評
-

## 次に読むべき論文
-
