---
title: "梶研 05/03 - 05/09"
date: 2023-05-09T20:00:00+09:00
weight: 20230509
tags: ["Python", "歩行軌跡"]
categories: ["活動報告", "梶研"]
---

# スケジュール
- [x] 3次元の歩行軌跡
  - [x] 計測
  - [x] 二次元グラフ化
  - [x] 三次元グラフ化(Python)
  - [x] 三次元グラフ化(Unity)


# 進捗
## 計測
4号館別館の1階(階段前)から3階(トイレ前)へ


### 使用機器など
- Android  Xperia
- Phyphox
  - 加速度センサー
  - 角速度センサー
  - 気圧センサー

## 二次元グラフ化
### 各センサーの値
![各センサーの値](images/output_2.png)

(前後40サンプルの移動平均フィルターをかけてある)  

### 二次元グラフ(加速度, 角速度)
![二次元グラフ](images/output_1.png)

1-2階と2-3階が1.5mほどずれている  
=> 階段の歩幅と平坦部の歩幅が異なるから?

## 三次元グラフ化(Python)
### 気圧から高さを出す
1気圧の環境で `1hPa` 下がると `10m` 上があるとして計算

### 三次元グラフ
![三次元グラフ](images/output_3.png)

### 三次元グラフ(横から)
![三次元グラフ](images/output_4.png)

階段を登っている様子はわかるが、沈んでしまっている(?)  
=> もっと平滑化すべき?

### 平滑化を強めた(?)グラフ
![平滑化を強めたグラフ](images/output_6.png)

![平滑化を強めたグラフ](images/output_5.png)

前後160サンプルで平滑化  

やりすぎだが、フィルター次第で綺麗にできる問題だと分かった


## 三次元グラフ化(Unity)
Pythonで座標をcsvで書き出して、c#で読み込む

[c#スクリプト](./lineGraph.cs)

少数16桁ある場合は `System.Globalization.NumberStyles.Float` が必要となる

```c#
float.Parse(line[0], System.Globalization.NumberStyles.Float);
```

![スクショ](images/screenshot_1.png)

動かしながら見れるようになった


# メモ
[kajilab.net 論文](https://kajilab.net/kaji/)
> 梶克彦，河口信夫，安定センシング区間検出に基づく3次元歩行軌跡推定手法， 情報処理学会論文誌，Vol.57, No.1, pp.12-24，2016. (特選論文)

直線や直角,並行な部分を割り出して綺麗にする研究  
長期的にみてやる


[情報処理学会](https://www.ipsj.or.jp/nyukai.html)
入っておくと良い

TODO:
- 直線や直角,並行な部分を割り出して綺麗にする
- Unity で時間通りのアニメーションにする