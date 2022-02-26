# AutoAugment:Learning Augmentation Strategies from Data

## 論文について (掲載ジャーナルなど)
- [Cubuk, E. D., Zoph, B., Mane, D.,  Vasudevan, V., & Le, Q. V. (2019). Autoaugment: Learning  augmentation strategies from data. In *Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition* (pp. 113-123).](https://arxiv.org/pdf/1805.09501.pdf)

## 概要
- データオーグメンテーションの方針を自動的に探索するシンプルな手順
- データオーグメンテーションのポリシーは多数のサブポリシーから構成される
    - 画像処理関数：並進、回転、せん断
    - 画像処理関数を適用する確率と、画像処理関数の強さ

- 複数データ・セットでSOTA(2019年時)

## 問題設定と解決したこと
- データオーグメンテーションは画像分類器の精度向上に効くけど、人手でやるのメンドイ
- 方針決めて、自動化しました

## 何をどう使ったのか
- 

## 主張の有効性の検証方法
- CIFAR-10、CIFAR-100、SVHN、ImageNetで調べたらSOTAだった
    - 精度 83.1%→83.5% @ImageNet
    - 誤り率 2.1%→1.5% @CIFAR-10

- 増強方針はデータセット間で移行が可能

## 批評
- [torchvisionのautoaugment](http://pytorch.org/vision/main/generated/torchvision.transforms.AutoAugment.html)の項目を見て、読もうと思った

## 次に読むべき論文
- 
