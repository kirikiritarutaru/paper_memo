# Do We Really Need Deep Learning Models for Time Series Forecasting?

## 論文について (掲載ジャーナルなど)
- [Elsayed, S., Thyssens, D., Rashed, A., Jomaa, H. S., & Schmidt-Thieme, L. (2021). Do we really need deep  learning models for time series forecasting?. *arXiv preprint arXiv:2101.02118*.](https://arxiv.org/pdf/2101.02118.pdf)

## 概要
- 時系列予測タスクだとGradient Boosting Regression Tree（GBRT）= XGBoost のほうがDNNよりも高性能やで
    - GBRTモデルのいい使い方を提案したやで


## 問題設定と解決したこと
- 時系列予測は幅広い応用がある→良い時系列予測モデル欲しいなぁ

    - 例）電力消費量、交通量、大気の状態の予測

- 時系列予測モデルたち

    - 従来
        - ローリングアベレージ、ベクトル自己回帰、自己回帰統合移動平均に依存したモデル

    - 最近

        - DNN、行列分解モデル
            - ↑は従来の手法と比較して、過度に複雑化しがち

- DNNモデルって、扱いにくいし本当に精度向上してんのか？

    - 精度よくてもっと扱いやすい手法ないかなぁ
    - 本当に、進歩してるのか確認の意味も込めて評価するやで

    

- リサーチクエスチョン

    1.  時系列予測のためのウィンドウベース学習フレームワークの観点から、GBRTモデルの入出力構造を構成した時の効果はいかほどか
    2.  うまく構成したGBRTモデルの実力はSOTAのDNNモデルたちと比較していかほどか

- コントリビューション

    1.  GBRTへの入出力構造を工夫して時系列予測モデルを作成
        -   GBRTをウィンドウベースの回帰フレームワークに落とし込んだ
        -   モデルの入出力構造を特徴付けることで、つよつよ時系列予測モデルを構築
            -   コンテキスト情報の負荷による恩恵を受けられるようにしたやで

    2.  ベースラインとの比較
        -   GBRTのウィンドウベースの入力設定が、ARIMAや素朴なGBRT実装など従来型の時系列予測モデルの精度を向上させることを実証的に証明
            -   時系列予測モデルにおける入力処理の重要性を強調するため

    3.  SOTAなDNNモデルと比較
        -   2種の時系列予測タスクにおける性能を調査
            -   単変量時系列予測タスク
            -   多変量時系列予測タスク

- 

## 何をどう使ったのか
- 

## 主張の有効性の検証方法
- 

## 批評
- [@JFPugetさんのツイート](https://twitter.com/JFPuget/status/1499739507905556484)
    - XGBoost better than deep learning for time series.  A paper confirming what ever Kaggler knows.

- [@tmaeharaさんのツイート](https://twitter.com/tmaehara/status/1499757003538894854)
    - NN が他モデルに負けてる話も，経験上，データ量を 100,000 倍にして 100,000 台のマシンで学習したら並ぶか逆転するので


## 次に読むべき論文
- 
