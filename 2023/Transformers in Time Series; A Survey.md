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
          - 時系列からある程度の位置情報を抽出することはできるが、重要な特徴を完全に利用することはできない
        - Learnable Positional Encoding
          - 時系列データから適切な positional encoding を学習することが効果的で、特定のタスクによくマッチする
            - [意見] ↑出典書け
        - Timestamp Encoding
          - カレンダータイムスタンプ（秒、分、時、週、月、年など）や特別なタイムスタンプ（祝日やイベントなど）は実アプリケーションではめっちゃ有効
            - あたりまえだよなぁ！
          - なのに、バニラ Transformer ではほとんど活用されない
            - もったいない
          - タイムスタンプ encoding の方法は、Autoformer や FEDformer で用いられている
      - Attention Module
        - アテンションモジュールは、「入力パターンのペア同士の類似度に基づいて動的に重みを変更する完全連結層」とみなせる
          - 利点：長期的な依存関係をモデル化するのに適している
            - （LSTMやRNNと比較して長期的なだけで、入力系列長が万ぐらいのオーダーになれば普通に無理だけどね）
          - 欠点：時間・メモリ複雑度が$O(N^2)$ （$N$ は系列長）←計算上ボトルネックになる
        - 欠点を解消しようとする工夫
          1. LogTrans, Pyraformer のようにアテンション機構に疎なバイアスを追加する方法
          2. Informer, FEDformer のように自己注意行列の低ランク特性を見つけて計算を高速化する方法
      - Architecture-based Attention Innovation
        - アテンションモジュール自体に対する工夫
          - Informer：アテンションモジュール間にストライド2のマックスプーリング層を挟んで、系列長を半分にダウンサンプリング
          - Pyraformer：C-ary tree ベースのアテンション機構を設計し、細かいスケールのノードは元の時系列に対応、粗いスケールのノードはより低い解像度の時系列に対応。異なる解像度にまたがる時間的依存性をよくとらえられる
    - Applications of Time Series Transformers
      - 時系列予測
        - ２つの観点で工夫が重ねられている
          - モジュールレベル（←2022年現在最新の研究は大部分がこっち）
            - 新しいアテンションモジュールの設計（6つ）
              -  LogTrans [Li et al., 2019], Informer [Zhou et al., 2021AST [Wu et al., 2020a], Pyraformer [Liu et al., 2022a], Quatformer [Chen et al., 2022], and FEDformer [Zhou et al., 2022]
                 - スパースの誘導バイアスor低ランク近似を用いて、計算複雑度を抑える改善
                 - 中でも、AutoformerとFEDformerが注目されている
                   - ↑フーリエ変換とウェーブレット変換を用いた周波数領域でアテンションを取り扱う手法
            - 時系列データの正規化方法の追求
              - Non-stationary Transformer [Liu et al., 2022b]
                - 定常/非定常モジュールを用いて、過定常化問題を抑制
                  - [意見] ↑意味不明
            - 入力トークンのバイアスの活用
              - Autoformer [Wu et al., 2021]
                - アテンションモジュールとして動く季節-トレンド分解アーキテクチャ
                  - 入力信号間の時間遅延の類似性を測定し、類似した部分系列を集約して複雑さを軽減した出力を生成
              - Crossformer [Zhang and Yan, 2023]
                - 多変量時系列予測のために、次元を超えた依存関係を利用したTransformerベースのモデル
                  - 入力信号を新しい次元区分別埋め込みによって2次元ベクトル配列に埋め込む
                  - 2段階のアテンション層を用いて、時空間依存性と次元間依存性を効率的に捉える
                    - [意見] Crossformer は面白そう。ただ、次元間の関係まで捉えないと解けない課題自体が少なそう。
          - アーキテクチャレベルのバリエーション
            - Triformer [Cirstea et al., 2022]
              - 多層の三角木型の構造を用いる方法
                - [意見] 決定木っぽいアーキテクチャということかな…？pros/consを書け
            - Scaleformer [Shabani et al., 2023]
              - 複数のスケールで共有された重みを繰り返し使う方法
          - 備考
            - 長期時系列予測にTransformerを適用するのがいいのかどうかはまだまだ調査段階
              - MLPベースのモデルがいいという報告もあるし、それよりも最新のTranformerをベースとしたPatchTSTの方が高性能という報告もある
              - [意見] どっちの論文もチェリーピッキングじゃないんですか？？？
          - 時空間予測
            - Traffic Transformer [Cai et al., 2020]
              - 時空間依存性を捉えるための自己注意モジュールと空間依存性を捉えるためのグラフニューラルネットワークモジュールを用いてエンコーダ・デコーダ構造を設計
            - Spatial-temporal Transformer [Xu et al., 2020]
              - 時間的依存性を捉えるための時間的Transformerブロックを導入
              - 空間的依存性をよりよく捉えるために、グラフ畳み込みネットワークとともに空間的Transformerブロックを設計
            - 他にも歩行者の軌跡予測とか、気象・気候予測とか、大気質予測とかに特化したTransformerベースの手法が提案されている
          - イベント予測
            - イベント予測とは？
              - 過去のイベントの履歴を考慮して将来のイベントの時間とそのイベントの種類を予測すること
              - 時間点過程（TPP）によってモデル化されることが多い
            - Self-attentive Hawkes process (SAHP) [Zhang et al., 2020]とTransformer Hawkes process (THP) [Zuo et al., 2020]
              - 過去のイベントの影響を要約してイベント予測のための強度関数を計算
            - attentive neural datalog through time (ANDTT) [Mei et al., 2022]
              - SAHPとTHPの拡張
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
  - 文献羅列してるわりにおそらく足りてないし、示唆をまとめているわけでもないし、中身を解説しようともしてない
    - いやまじで解説しろコラ
  - 全然包括的に感じないが？？

## 次に読むべき論文
- なし
