---
title: "シス研 webサイト"
date: 2023-10-30T15:30:51+09:00
weight: 20231030
tags: ["webサイト", "Next.js"]
categories: ["活動報告", "シス研"]
---

私が所属する システム工学研究会 のwebサイトを制作しました。

[sysken.net](https://www.sysken.net/)

## 技術構成
フロントエンドは Next.js(pages router) を使用しています。
また、シス研でドキュメントを残すために使用している [esa](https://esa.io/) というサービスを CMS にし、
SSGとして運用しています。

## 工夫点
無駄にデスクトップ感を出すために Web API を使用して、メニューバーの充電量をリアルタイムで表示しています。
(safari等は未対応)

## 感想
自分のこのブログも Next.js で制作しようとしていました。
しかしファイルを取得し各ページにビルドするというのがうまくいかず、結局 HUGO で制作しました。

シス研のサイトを制作する上で改めて挑戦したところ簡単に実装でき、一ヶ月の差でも成長を感じました。
