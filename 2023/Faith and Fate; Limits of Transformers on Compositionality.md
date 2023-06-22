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
    - 複雑さのレベルを体型定期に定量化するため、compositional tasksを計算グラフとして定式化
    - Transformers の compositional tasksを解く方法の洞察
      - 体系的な問題解決スキルを獲得しているわけではない
      - 多段階の構成的推論を線形化された部分グラフのマッチングに還元することで、compositional tasksを解いている（ようだ）
    - タスクの複雑性が増加するにしたがい、Transformersのパフォーマンスが急速に低下する実験結果

## 問題設定と解決したこと
-

## 何をどう使ったのか
-

## 主張の有効性の検証方法
-

## 批評
-

## 次に読むべき論文
-
