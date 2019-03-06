---
layout: post
title:  "Jupyter notebook + RISE で講義をする"
description: "Jupyter notebook でプレゼンをする方法を紹介します"
date:   2018-06-13 01:17:42 +0900
tags: [software, python, jupyter notebook, rise]
share: true
---

## モチベーション
機械学習+プログラミングの講義をやることになった（全15回）。
つい勢いで、 Python を教えながら機械学習のアルゴリズムも教えて、なんならアルゴリズムの実装も行う、みたいな授業計画にしてしまった。

ここで困るのが授業の進め方である。当然スライドを用いて講義をすることになるのだが、 PowerPoint などで講義するイメージができない。
スライドと Jupyer notebook を行き来して講義をしようかと考えていたのだが、 Jupyter notebook でプレゼンテーション自体もできるらしいということがわかったので、
そのやり方をいろいろ調べてみた。それをまとめたい。

## [RISE](https://github.com/damianavila/RISE)のインストール
人はだいたい Anaconda を使っていると思うので、
```
conda install -c damianavila82 rise
```
でインストール。

## 基本的な使い方
Jupyter notebook を開くと、下の図の赤で囲ったようなボタンができているはず。
あと Markdown のセルを作ると、 Slide Type というのが選べるはず。

![Jupyter notebook 起動画面]({{site.baseurl}}/images/2018-06-14/rise_overview.png)

1. Markdown のセルを作って Slide Type で `Slide` を選択
   - 中でコードを見せたいときには次のセルで普通にコードを書く
   ![セル]({{site.baseurl}}/images/2018-06-14/rise_cell.png)
   - Sub-Slide を使うと下に移動できるようになる。大きなテーマは Slide で書いて、その細分を Sub-Slide で書くといいかも。
1. 普通の Markdown の記法でスライドの中身を書く
1. 上の図の赤で囲ったボタンを押すとスライドショーが始まる

が基本的な流れです。こんな感じのスライドショーになります。

![スライドショー]({{site.baseurl}}/images/2018-06-14/rise_slideshow.png)


## 設定
`Edit >> Edit Notebook Metadata` の `livereveal` で RISE の設定ができます。
![スライドショー]({{site.baseurl}}/images/2018-06-14/rise_config.png)

[ここ](https://damianavila.github.io/RISE/customize.html) にいけば設定できる内容が見られる。

- `theme` はちゃんと設定できるものとそうでないものがあるっぽい？
- `scroll` は `true` にしたほうがいい（コードのセルはスクロールできないとさすがに厳しい）


### 見た目を変えたい
なんか字が大きかったりとか、見た目がダサいなあというときには css でなんとかしないといけない。
特にいじりたかったのは
- フォントサイズ
- テキストを上揃えにしたい
- H1で書いたときにタイトル感を出したい（色とか）
- テーブル...

#### CSS ファイルの準備
私は[こんな感じ](https://github.com/kanojikajino/lecture/blob/master/custom.css)のCSSファイルを作った。


#### CSS ファイルの利用
notebook のはじめにコードのセルを作り、

```
%%HTML
<link rel="stylesheet" type="text/css" href="path/to/css">
```

と書くと CSS が適用される。

## 実例
[ここ](https://github.com/kanojikajino/lecture)で資料作ってます。誰か作ってください...。
