# Image Segmentation Using Deep Learning: A Survey

## 論文について (掲載ジャーナルなど)
- [Minaee, S., Boykov, Y. Y., Porikli, F., Plaza, A. J., Kehtarnavaz, N., & Terzopoulos, D. (2021). Image segmentation using deep learning: A survey. *IEEE transactions on pattern analysis and machine intelligence*.](https://arxiv.org/pdf/2001.05566.pdf)

## 概要
- Image Segmentation は下記の多くのアプリケーションをもつコンピュータビジョンにおける重要なトピック
    - シーン理解、医療画像解析、ロボット近く、ビデオ監視、拡張現実、画像圧縮

- ディープラーニングは近年、 Image Segmentation 分野で成功を収めている
- 本サーベイでは、Image Segmentation 分野におけるディープラーニングの包括的レビューを提供する

## 問題設定と解決したこと
- 本論文のポイント
    - ディープラーニング（DL）を用いた画像のセグメンテーションについて包括的にまとめた
        - 2019年までの100個以上のDLモデルについて紹介し、10のカテゴリーに分類して概観する

    - DLを用いたセグメンテーションアルゴリズムを以下の側面で分析する
        - 学習データ
        - ネットワークアーキテクチャの選択
        - 損失関数
        - 学習戦略
        - 主要な貢献

    - 2D, 2.5D(RGBD), 3D画像に分類された、約20個の一般的な画像セグメンテーションデータ・セットの概要をまとめた
    - 一般的なベンチマークにおける、各手法の特性と性能の比較
    - DLに基づくセグメンテーションの課題と今後の方向性を示した

- そもそもセグメンテーションとは？
    - 画像を複数のセグメントやオブジェクトに分割すること
    - 幅広いアプリケーションで中心的な役割を果たしている
        - シーン理解、医療画像解析、ロボット近く、ビデオ監視、拡張現実、画像圧縮

- セグメンテーションの種類は？
    - セマンティックセグメンテーション
        - 意味的なラベルを持つピクセルの分類問題
        - すべての画像ピクセルに対して、オブジェクトカテゴリー（例：人間、車、木、空）のセットでピクセルレベルのラベリングを行うこと

    - インスタンスセグメンテーション
        - 個々のオブジェクトの分類問題
        - 画像の各注目オブジェクト（例：個々の人物の分割）を検出し、区切ること

- 以下の10+1パターンにわけてDLを用いたセグメンテーションモデルを分類した
    1.   Fully convolutional networks 
    2.   Convolutional models with graphical models 
    3.   Encoder-decoder based models
    4.   Multi-scale and pyramid network based models 
    5.   R-CNN based models (for instance segmentation) 
    6.   Dilated convolutional models and DeepLab family 
    7.   Recurrent neural network based models
    8.   Attention-based models 
    9.   Generative models and adversarial training 
    10.   Convolutional models with active contour models 
    11.   Other models

-   ネットワークアーキテクチャの概観
    1.  CNN
        -   特にコンピュータビジョンタスクで最も成功していると行ってもいいアーキテクチャ
        -   CNNアーキテクチャの概要
            -   畳み込み層：特徴を抽出するために、重みカーネルを畳み込む
            -   非線形層：ネットワークによる非線形関数のモデル化を可能にするために、特徴マップに活性化関数を適用する
        -   多解像度ピラミッドを形成するように層を積み重ねることで、上位の層は広い画像領域から特徴を学習する
        -   CNNの主な計算上の利点は、「一つの層内の」
    2.  RNNとLSTM
        -   時系列データを処理するために広く使用されているアーキテクチャ
        -   任意の時間/位置のデータは、その時点以前に遭遇したデータに依存する
        -   RNNは理論的にはデータの長期的な依存関係を捉えることができるはずだが、勾配消失/勾配爆発の問題がありアプリケーションとしてはうまく動かない
        -   LSTMは上記の問題を回避するように設計されているが、現時点（2023年）ではアプリケーションとしてうまく動いている例を私は知らない
    3.  Encoder-Decoder and Auto-Encoder Models
        -   エンコーダ・デコーダモデルは、入力領域から出力領域へのデータ点のマッピングを、2段階のネットワークを介して学習するモデル群
            -   エンコーダ：入力データを潜在空間表現に圧縮
                -   潜在空間表現：基本的に特徴表現のことであり、出力を予測するのに有用な、入力の根底にある意味情報を汲み取った表現
            -   デコーダ：潜在空間表現から出力を予測
        -   このモデルは再構成損失を最小化することによって学習される
        -   オートエンコーダーモデル：入力と出力が同じであるエンコーダ・デコーダモデルの特殊なケース
    4.  GAN
        -   生成器と識別器の2つのネットワークから構成
            -   生成器：事前分布を持つノイズから「本物」に類似したターゲット分布へのマッピングを学習
            -   識別器：生成されたサンプル「偽物」から「本物」を区別しようとする
        -   [意見] 学習が難しい。生成されたものの評価が難しい。最近（2023年）は、Diffusion Modelに話題を取られがち。
-   DLモデルの概観
    1.  Fully Convolutional Networks
    2.  Convolutional Models with Graphical Models
    3.  Encoder-Decoder based Models
    4.  Multi-Scale and Pyramid Network Based Models
    5.  R-CNN Based Models for Instance Segmentation
    6.  Dilated Convolutional Models and DeepLab Family
    7.  Recurrent Nerral Network Based Models
    8.  Attention-Based Models
    9.  Generative Models and Adversarial Training
    10.  CNN Models With Active Contour Models
    11.  Other Models

## 何をどう使ったのか
-   

## 主張の有効性の検証方法
- 

## 批評
- 

## 次に読むべき論文
- 
