---
layout: post
title:  "「化学構造式のためのハイパーグラフ文法」を発表した（JSAI2018）"
description: "化学構造式を生成するための文脈自由文法を作りました"
date:   2018-06-19 01:11:00 +0900
tags: [research, cheminformatics, generative model, graph generation, grammar]
---

2018年度人工知能学会全国大会で「[化学構造式のためのハイパーグラフ文法](https://confit.atlas.jp/guide/event-img/jsai2018/3E1-04/public/pdf?type=in)」という論文 [1] の発表を行った。
以下に発表に用いたスライドと、スライドの簡単なまとめを書く。

<iframe src="//www.slideshare.net/slideshow/embed_code/key/sBM6eOlfbGFKm1" width="595" height="485" frameborder="0" marginwidth="0" marginheight="0" scrolling="no" style="border:1px solid #CCC; border-width:1px; margin-bottom:5px; max-width: 100%;" allowfullscreen> </iframe> <div style="margin-bottom:5px"> <strong> <a href="//www.slideshare.net/kanojikajino/jsai2018-102589449" title="化学構造式のためのハイパーグラフ文法（JSAI2018）" target="_blank">化学構造式のためのハイパーグラフ文法（JSAI2018）</a> </strong> from <strong><a href="https://www.slideshare.net/kanojikajino" target="_blank">Hiroshi Kajino</a></strong> </div>

# __問題意識__
分子の構造式の生成モデル $$ p(G \mid \mathbf{z}) $$ を作りたい（ $$ G $$ は構造式、 $$ \mathbf{z} \in \mathbb{R}^{D} $$ は構造式に対応する潜在ベクトル）。

既存手法 [2] では、構造式のグラフ表現 $$ G $$ （原子＝ノード、結合＝エッジ）を直接生成するのではなく、構造式のテキスト表現である SMILES を LSTM などの系列モデルで生成した後に、 SMILES の文法に従ってグラフを生成する。
しかしこの手法は２つの欠点がある:

1. グラフの環や枝分かれをテキスト表現するために SMILES の文法は少し複雑になっている（SMILES は文脈依存文法）
1. SMILES の文法的に正しくても、化学的に正しい構造式ができるとは限らない。特に原子価を守っていない構造式が出てくることがある
   - SMILESを用いると、例えばフェノールは c1ccc(O)cc1 のように書ける。数字は環の始点・終点を表し、括弧は枝分かれを表す
   - 大きい分子になると括弧や数字の対応付けが分かりづらいし、原子価が制約を満たしているのかが分かりづらい

# __方針__
SMILES を他の文法に置き換える。特に

1. 文法が簡単であってほしい（学習が簡単になることが期待される）
1. 文法に従って生成した構造式は、原子価が必ず守られてほしい

の２つを満たすような文法だと嬉しい。
特に文脈自由文法であれば、 grammar variational autoencoder [3] のような手法を用いることで文法に従って構造式を生成することができる。


# __本研究の貢献__
分子のデータセットを入力したときに、上記の性質を満たす hyperedge replacement grammar （HRG; ハイパーエッジ置換文法; グラフを生成する文脈自由文法の一種）を構成するアルゴリズムを開発した。
ハイパーグラフの木分解を元に HRG を構成するアルゴリズムは知られていた [4] が、必ず原子価を守るグラフを生成できる HRG を構成するアルゴリズムを与えたことが本研究の貢献となる。
技術的には、

1. 分子をハイパーグラフを用いて表すこと（原子＝ハイパーエッジ、結合＝ノード）
1. ハイパーグラフの非冗長な木分解を用いること

の２点の工夫により、必ず原子価を守る構造式を生成することが証明できる。


# __参考文献__


[1] 梶野 洸: 化学構造式のためのハイパーグラフ文法, 人工知能学会全国大会, 2018, https://confit.atlas.jp/guide/event-img/jsai2018/3E1-04/public/pdf?type=in .

[2] Rafael Gómez-Bombarelli, Jennifer N. Wei, David Duvenaud, José Miguel Hernández-Lobato, Benjamín Sánchez-Lengeling, Dennis Sheberla, Jorge Aguilera-Iparraguirre, Timothy D. Hirzel, Ryan P. Adams, and Alán Aspuru-Guzik: Automatic Chemical Design Using a Data-Driven Continuous Representation of Molecules, ACS Central Science, 2018.

[3] Matt J. Kusner, Brooks Paige, and José Miguel Hernández-Lobato: Grammar variational autoencoder, ICML-17, 2017.

[4] Salvador Aguiñaga, Rodrigo Palacios, David Chiang, and Tim Weninger: Growing graphs from hyperedge replacement graph grammars, CIKM-16, 2016.

