# AdaCos: Adaptively Scaling Cosine Logits for Effectively Learning Deep Face Representations

## 論文について (掲載ジャーナルなど)
- [Zhang, X., Zhao, R., Qiao, Y., Wang,  X., & Li, H. (2019). Adacos: Adaptively scaling cosine logits for  effectively learning deep face representations. In *Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition* (pp. 10823-10832).](https://arxiv.org/abs/1905.00292)

## 概要
- ArcFaceはscaleとmarginの2つのハイパラに敏感（少し変えるとスコアが大きく変わる）
- 2つのハイパラの予測確率に対する影響を詳細に分析し、新しい湖西ベースのソフトマックスロスAdaCosを提案

## 問題設定と解決したこと
- コサインベースのソフトマックス損失（とその派生）は深層学習ベースの顔認識（←だけじゃないけど）で大きな成功を収めている
- 課題：これら損失におけるハイパラの設定が難しい
    1.  ハイパラ調整によって、最終的な認識性能が大きく変わる
    2.  最適化の経路もがっつり変わる
    3.  手動でチューニングするには、ユーザーの職人芸が必要
- 本論文では、以下のコントリビューションがある
    - 2つの重要なハイパラ、① scale パラメータと② angular margin パラメータの効果を詳細に分析
    - 上記分析に基づき、新しいコサインベースソフトマックス損失 AdaCosを提案
    - AdaCosを大規模顔検証・識別データセットで性能を検証し、学習が安定すること・高い顔認識精度を達成できることを確認

-   数式の定義

    -   $\vec{x}_i$ をサイズ$N$のみにバッチの$i$番目の顔画像の特徴 (deep representation) とし、対応するラベルを$y_i$とする

    -   ミニバッチ内の$N$個のサンプルの予測分類確率を $P_{i,j}$  とし、以下で計算する

        -   $$
            P_{i,j}=\frac{e^{f_{i,j}}}{\sum^C_{k=1}e^{f_{i,k}}}
            $$

            -   ここで、$f_{i,j}$をソフトマックスの入力として使われるロジット、$P_{i,j}$は$\vec{x}_i$をクラス$j$に割り当てるソフトマックス正規化確率、$C$はクラス数である。

    -   現在のミニバッチに関連するクロスエントロピー損失 $\mathcal{L}_{\text{CE}}$ は

        -   $$
            \mathcal{L}_{\text{CE}} = - \frac{1}{N}\sum^N_{i=1}\log P_{i,y_i} = -\frac{1}{N}\sum^N_{i=1}\log \frac{e^{f_{i,y_i}}}{\sum^C_{k=1}e^{f_{i,k}}}
            $$

        -   最新のコサインベースソフトマックス損失では、ロジット$f_{i,j}$の計算方法に工夫がある

            -   従来の方法：
                -   コサイン類似度：$\cos\theta_{i,j} = \frac{\langle \vec{x}_i, \vec{W}_j\rangle}{\|\vec{x}_i\|\|\vec{W}_j\|}$
                -   ロジット：$f_{i,j}=s\cdot\cos\theta_{i,j}$
                    -   $s$ は scale ハイパーパラメータ
            -   *ArcFace*：
                -   ロジット：$f_{i,j}=s\cdot\cos(\theta_{i,j}+\mathbb{1}_{\{j=y_i\}}\cdot m)$
                    -   $m$ はmargin ハイパーパラメータ
                    -   $\mathbb{1}_{\{\}}$ は指示関数
                -   注記：
                    -   （マージン**なし**の損失と比較して、）マージンを加えたロジットの計算方法では、クラスの割当があっているロジット $f_{i,y_i}$ に対して、クラスの割当があっていないロジット $f_{i,j}\ (j\not=y_i)$ よりも指定した $m$ 分だけ大きくなることを要求している。


-   直感的なscale パラメータ$s$と angular margin パラメータ$m$の説明
    -   $s$ はコサイン距離の狭い範囲を拡大し、ロジットがより識別力を持つようにする
    -   $m$ は異なるクラス間のマージンを拡大し、分類能力を向上させる
        -   これらのハイパラは最終的に予測確率に影響を与える
        -   理想的なハイパラ設定は
            1.  各クラスの予測確率は範囲 $[0,1]$ に及ぶべき。つまり、予測確率の lower bundary が0付近に、upper boundary が1付近にあるべきってこと。
            2.  学習を有効にするために、予測確率の変化のカーブはそのクラスの角度が大きく変わるべき

-   Scale パラメータ$s$の分析
    - 結論：
        - $s$ が小さすぎると、クラス予測確率の最大値が制限される
        - $s$ が大きすぎると、ほとんどのクラス予測確率が1になってしまい、学習損失が角度の正しさに依存しなくなる
            - $s$ が大きい損失関数は誤分類されたサンプルにペナルティを与えることができず、誤りを修正するために、ネットワークを効果的に更新できない可能性がある

-   angular margin パラメータ$m$の分析
    - 結論：
        - $m$ が小さすぎると最終的な角度マージンを正則化する力が弱くなる
        - $m$ が大きすぎると学習が収束しくくなる

    

## 何をどう使ったのか
- 本論文では、スケールパラメータ$s$ を自動的に調整することに焦点を当てる

- AdaCos
    - 学習サンプルと対応するクラス中心ベクトル（ソフトマックス前の完全連結ベクトル）のコサイン類似度を動的にスケーリングし、その予測クラス確率をコサイン類似度の意味するところに合致させることが可能

- AdaCosの思想

    - スケールパラメータ$s$を調整することで、クラス予測確率$P_{i,y_i}$の”曲率”を圧縮/伸張する

        - ”いい感じ”のところにクラス予測確率が変化する場所を持ってくる

        - $$
            P_{i,j} = \frac{e^{\bar{s}\cdot\cos\theta_{i,j}}}{\sum^C_{k=1}e^{\bar{s}\cdot\cos\theta_{i,k}}}
            $$

            -   ここで$\bar{s}$は自動で調整されたスケールパラメータ

    - Qどうやって$\bar{s}$をみつける？→A微分ですね！

        - 予測確率$P_{i,y_i}$が$\theta_{i,y_i}$に対して大きく変化するような適切なスケール$\bar{s}$を見つけたい

        - 絶対勾配値$\left\|\frac{\partial P_{i,y_i}(\theta)}{\partial\theta}\right\|$が最大になる角度を見つけたい

        - それは、二階微分が0になるような角度デス

        - $$
            \frac{\partial^2P_{i,y_i}(\theta_0)}{\partial\theta_0^2}=0
            $$

            -   ここで、$\theta_0\in[0,\frac{\pi}{2}]$

    - $P(\theta_0)$が$\frac{1}{2}$に近いとすると、スケールパラメータ$s$と点$(\theta_0,P(\theta_0))$の関係は次のように近似できる

    - $$
        s_0=\frac{\log B_i}{\cos\theta_0}
        $$

        - ここで、学習時に角度$\theta_{i,k}$が$\frac{\pi}{2}$付近に分布するため、$B_i=\sum_{k\not=y_i}e^{s\cdot\cos\theta_{i,k}}\approx C-1$とよく近似できる
        - $\bar{s}$を自動決定する作業は$[0,\frac{\pi}{2}]$における妥当な中心角$\tilde{\theta}$を決定する作業になる

    - *Automatically choosing a fixed scale parameter*

        - $\frac{\pi}{4}$は$[0,\frac{\pi}{2}]$の中心にあるので、$\theta_0=\frac{\pi}{4}$をとして、角度$\theta_{i,y_i}$からクラス予測確率$P_{i,y_i}$への有効な写像を考えることができる

        - 対応するスケールパラメータ$s_f$を次のように推定することができる

        - $$
            \bar{s}_f=\frac{\log B_i}{\cos\frac{\pi}{4}} = \frac{\log\sum_{k\not=y_i}e^{s\cdot\cos\theta_{i,k}}}{\cos\frac{\pi}{4}}\approx\sqrt{2}\cdot\log(C-1)
            $$

            

        - **注記**：

            - 実装をみるとスケールパラメータは上の式で自動決定で残ったマージンパラメータは$m=0.50$をデフォルトとして設定されてるっぽい
            - 参考：[4uiiurz1/pytorch-adacos](https://github.com/4uiiurz1/pytorch-adacos)



## 主張の有効性の検証方法
- クラスごとにスケールパラメータが固定の場合と適応的に変える場合でAdaCosを検証
    - 当然、適応的に変えるほうが精度は高い

- Fixed scale parameter
- Dynamically adaptive scale parameter

## 批評
- [Kaggle Happywhale – Whale and Dolphin Identificationで優勝&10位でソロ金メダルを獲得しました](https://tech.preferred.jp/ja/blog/kaggle-happywhale-1st-10th-solution/)で知った
- Kaggleでは、ArcFaceのscaleパラメータとangular marginパラメータをクラスごとに適応的に変化させる方法がよく使われる
    - クラスごとサンプル数が不均衡な場合に同じパラメータ使うのはちがくね？という洞察


## 次に読むべき論文
- 
