---
title: "出席確認システムを作った-2"
date: 2022-10-22T22:11:55+09:00
weight: 20221022
tags: ["Python", "Raspberry Pi", "M5stack", "Pasori", "Google Apps Script"]
categories: ["活動報告", "システム"]
---

[出席確認システムを作った](/posts/activity-report/2022-06/attendanse-system/) の後に、バグが多かったため作り直しました

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

---

#### バグ / 問題
- 謎のエラーがよく出る
- 毎日0時に消されるはずのデータベースが消されてない
- 帰りの時間を知らせる音楽を ON/OFF できない
- 反応が遅い

ということで...作り直しました。

#### 変更点
ver3.0 でのプログラム
- check.py
  - NFCタグを検知する
  - 判別後スプレッドシートとデータベースを更新
  - スプレッドシートがなかった場合のシートを追加・更新
- periodic.py
  - 設定時間を取得し、その時間に音楽を流す
  - 毎月スプレッドシートのシートを追加・更新
  - 0時にデータベースを削除

ver3.2 でのプログラム
- check.py
  - NFCタグを検知する
  - 判別後スプレッドシートとデータベースを更新
  - ~~スプレッドシートがなかった場合のシートを追加・更新~~
  - その日以外のデータベースを削除
- periodic.py
  - 設定時間を取得し、その時間に音楽を流す
  - ~~毎月スプレッドシートのシートを追加・更新~~
  - ~~0時にデータベースを削除~~
- Google Apps Script
  - スプレッドシートのシートを追加・更新

#### 説明
データベースの管理を２つのファイルを跨いでいたため複雑になっていました。  
そこで `check.py` だけで管理させることにより簡略化させました。

帰りの時間を知らせる音楽を `ON/OFF` できないことに対して、曜日ごとに時間と `ON/OFF` を設定できるようにしました

タッチしてからスプレッドシートからデータを取得していて、音が鳴るまで時間がかかっていました。  
タッチ後にスプレッドシートのデータを取得・更新することにより、タッチから音が鳴るまでの処理はローカル内で済み、反応が早くなりました。

毎月の記録シートの追加を `Python` からやっていたため失敗した場合の処理が必要だったりしました。　　
そこで `AppsScript` を使うことにより失敗を減らし、また `Python` からのアクセス数を減らすこともできました。