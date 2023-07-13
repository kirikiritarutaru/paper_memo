# Neural Reprojection Error: Merging Feature Learning and Camera Pose Estimation

## 論文について (掲載ジャーナルなど)
- [Germain, H., Lepetit, V., & Bourmaud, G. (2021). Neural reprojection error: Merging feature learning and camera pose estimation. In Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition (pp. 414-423).](https://arxiv.org/abs/2103.07153)

## 概要
- 絶対カメラポーズ推定は通常次の2つのサブ問題を解くことで解く
  - 2D-3D対応の推定を行う特徴マッチング問題
  - カメラポーズに関して、再投影誤差の和を最小化するPnP問題
- 本論文では、再投影誤差に代わるものとして、ニューラル・リプロジェクション・エラー（NRE）を提案
  - NREの再投影誤差よりも良いポイント
    - 2D-3D対応よりも豊富な情報を活用できる
    - ロバストな損失
    - ハイパーパラメータ選択をなくせる
- NRE項の和を効率的に最小化する最適化手法も提案
- 計算効率とメモリ効率を高めたまま、カメラ姿勢推定のロバスト性と精度の両方を改善

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
