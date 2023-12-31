---
title: "出席確認システムを作った"
date: 2022-06-12T20:51:24+09:00
weight: 20220610
tags: ["Python", "Raspberry Pi", "M5stack", "Pasori"]
categories: ["活動報告", "システム"]
---


#### 概要
1. 情報デザイン部 → システムコンピュータ部 に転部
2. まずは出席を... ***まさかの紙 !?***
3. →デジタル化しよう
<!--more-->
<br>

---
## 出席確認システム ver1.0
<br>

#### 使用機器等
- M5stack Core2
- 指紋センサユニット
- Raspberry Pi 4B 4GB
    - Apache2

#### システム
1. M5stackが指紋認識
2. ラズパイに HTTPPost で送信
3. csvファイルにログを保存

- webサイトから管理

<br>

#### メリット
- **ロマン**
- 指紋のため何かを持ち歩く必要がない

#### デメリット
- 指紋の認識精度が低い
- 登録がとても大変

<br>

[出席確認システム ver1.0 詳細](/notyet/)

<br>

---
## 出席確認システム ver2.0
#### 使用機器等
- Raspberry Pi 4B 4GB
- Pasori
- Slackbot
    - SlackBolt

#### システム
1. Pasori からNFCタグを認識
2. csvファイルにログを保存

- Slackから管理

<br>

#### メリット
- 指紋のため何かを持ち歩く必要がない

#### デメリット
- 管理を全てSlackで行うため追加登録・変更が大変
- 管理が大変

<br>

[出席確認システム ver2.0 詳細](/notyet/)

<br>

---
## 出席確認システム ver3.0
#### 使用機器等
- Raspberry Pi 4B 4GB
- Pasori
- Google スプレッドシート
    - GSpread

#### システム
1. Pasori からNFCタグを認識
2. Googleスプレッドシートに記録

- Googleスプレッドシートから管理

<br>

#### メリット
- スプレッドシートの機能を使うため管理がしやすい
- 持続性がある

#### デメリット
- 通信量に制限がある

<br>

[出席確認システム ver3.0 詳細](/notyet/)

<br>