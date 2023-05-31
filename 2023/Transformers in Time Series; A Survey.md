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
  - 時系列モデリングにTransformerを効果的に使用する方法について実用的なガイドラインを提供

- Transformer の特徴
  - Positional Encoding
    - バニラ Transformer は再帰性を持たないかわりに、入力埋め込みに追加された位置符号化を利用して配列情報をモデル化する
    - 位置符号化2種類
      - 絶対位置符号化
        - 位置インデックス $t$ にっついて、
        - エンコーディングベクトルは
          - $$\begin{equation} PE(t)_i= \left \{ \begin{array}{l} \sin(\omega_i t)\qquad (i\%2=0) \\ \cos(\omega_i t)\qquad(i\%2=1) \end{array} \right.  \end{equation} $$
      - 相対位置符号化
        - 入力要素間のペア的な位置関係のほうが有益なケースで用いる
        - [Shaw etal., 2018](https://arxiv.org/abs/1803.02155)参照

  - Multi-head Attention
    - クエリ $\bold{Q}\in\mathbb{R}^{N\times D_k}$、キー $\bold{K}\in\mathbb{R}^{M\times D_k}$、バリュー $\bold{V}^{M\times D_v}$（$N,M$ は、クエリとキーの長さ、$D_k, D_v$ はキー、クエリの次元数）とする
    - $\text{Attention}(\bold{Q}, \bold{K}, \bold{V}) = \text{softmax}\frac{\bold{Q}\bold{K}^\mathsf{T}}{\sqrt{D_k}}\bold{V}$

  - Feed-forward and Residual Network
    - 省略

## 何をどう使ったのか
- 時系列Transformersの分類
  - 図1のような分類法を提案
    - Network Modifications for Time Series
      - Positional Encoding
        - Vannila Positional Encoding
        - Learnable Positional Encoding
        - Timestamp Encoding
      - Attention Module
      - Architecture-based Attention Innovation
    - Applications of Time Series Transformers
      - 時系列予測
      - 異常検知
      - 分類
- 時系列Transformersの評価
  - ロバスト性解析
  - モデルサイズ解析
  - 季節-トレンド分解解析

## 主張の有効性の検証方法
-

## 批評
- 駄目論文だった
  - 文献羅列してるわりにおろらく足りてないし、示唆をまとめているわけでもないし、中身を解説しようともしてない
  - 全然包括的に感じないが？？

## 次に読むべき論文
- なし
