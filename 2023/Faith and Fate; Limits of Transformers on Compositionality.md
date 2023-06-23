# Faith and Fate: Limits of Transformers on Compositionality

## 論文について (掲載ジャーナルなど)
- [Nouha Dziri, Ximing Lu, Melanie Sclar, Xiang Lorraine Li, Liwei Jiang, Bill Yuchen Lin, Peter West, Chandra Bhagavatula, Ronan Le Bras, Jena D. Hwang, Soumya Sanyal, Sean Welleck, Xiang Ren, Allyson Ettinger, Zaid Harchaoui, Yejin Choi](https://arxiv.org/abs/2305.18654)

## 概要
- 近年、LLMは大きな注目を集め、特に複雑な多段階の推論を必要とするタスクで高性能を発揮することがよく知られている
- しかしながら、驚くほど些細な問題で失敗する場合もある
  - 「なぜ失敗するのか」について検討した
- 本論文は、以下の4つのコントリビューションがある
    - LLMを解明する試みとして、3つの代表的な compositional tasks においてモデルの限界を調査
        - 桁の多い数字の掛け算、ロジックグリッドパズル、古典的な動的計画問題
        - compositional tasks =「問題をサブステップに分解し、それらのステップの結果を合成して正解を導く」ような問題
    - 複雑さのレベルを体系的に定量化するため、compositional tasksを計算グラフとして定式化
    - Transformers の compositional tasksを解く方法の洞察
      - 体系的な問題解決スキルを獲得しているわけではない
      - 多段階の構成的推論を線形化された部分グラフのマッチングに還元することで、compositional tasksを解いている（ようだ）
    - タスクの複雑性が増加するにしたがい、Transformersのパフォーマンスが急速に低下する実験結果

## 問題設定と解決したこと
- 問題設定
  - LLM の compositional tasks を解く能力の限界を知りたい
    - LLMは、いろんなタスクで高性能を発揮する一方、簡単な $3\times3$ の計算の正解率は55%~59% ←なんで？？
- 仮説
    1. Transformers は多段階の構成的推論を線形化されたパスマッチングに還元することにより、compositional tasksを解く
       - パターンマッチングによるショートカット学習は、トレーニング中に類似の構成パターンが利用可能な場合、高速な正解を得ることができる
       - 一般的でない例や複雑な例に対するロバストな汎化はできない
    2. エラーの伝播により、Transformers は新しいパターンの高複雑度の compositional tasks を解くことは本質的な限界がある
       - 計算プロセスの初期段階でのエラーが後続のステップにおける複合エラーにつながり、モデルが正解を見つけることを妨げるだろう

- 解決したこと
  - compositional tasks の複雑さのレベルを体系的に定量化する方法を提案
  - LLMの推論力を調べた
    - LLMの推論力の傾向
      - タスクに特化したデータを用いた学習は、ドメイン内のインスタンスや低い構成複雑度のもとではほぼ完璧な性能を示す
      - この領域外のインスタンスでは大きく失敗する
    - LLMのドメインの領域内外でのギャップにより以下のことが示唆される
      - **人間のような推論ステップを促したり訓練したとしても、入出力のシーケンスを尤もらしく繋げる方法（maximum likelihood training）からは、体系的な問題解決能力が生まれないのではないか**
  - LLMの失敗するケースの分析の結果
    - **モデルは単一ステップの演算を記憶することはできるが、それらを正しい推論パスに構成することはできず、深い全体的な課題理解ではなく、浅い暗記学習に基づいて予測を行うことがほとんど！**

## 何をどう使ったのか
-

## 主張の有効性の検証方法
-

## 批評
-

## 次に読むべき論文
-