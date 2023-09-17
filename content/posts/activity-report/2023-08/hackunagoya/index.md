---
title: "Open Hack U 2023 NAGOYA"
date: 2023-08-26T18:00:00+09:00
weight: 20230826
tags: ["Open Hack U 2023 NAGOYA", "Next.js", "SCSS"]
categories: ["活動報告", "webアプリ", "ハッカソン"]
---

# 概要
Yahoo が主催する `Open Hack U 2023 NAGOYA` に参加しました。  
3週間の事前開発ののち、Yahoo名古屋オフィスにてオフラインで発表をしました。

研究室の先輩と5人でチームを組み、`Mogy` という Markdown で簡単に投稿し、動画として閲覧できるサービスを作成しました。

# 結果
**最優秀賞** をとれた！

IT系だけでなく他の業界でも応用が聞いて良いとのことでした。

# 発表の様子
<iframe width="560" height="315" src="https://www.youtube.com/embed/uI0KZHE8qAg?si=O--jq9lJf7n-Klsk&amp;start=2522" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# 結果発表の様子
<iframe width="560" height="315" src="https://www.youtube.com/embed/uI0KZHE8qAg?si=9V2XmUzSWFsyefp-&amp;start=10286" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

# 発表スライド
<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRuqeCspR7sKMl35oS5SppyzgdpkSGlv-pd3PqUIKttIFcsCW_sQDY7lmfDj5BPKOOxjOBHOAz3p3vh/embed?start=false&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

# 自分の担当
- アプリデザイン
- フロントエンド(閲覧, 投稿, 検索ページ)
- 発表スライド/原稿
- 当日の発表

# 発表スライド
<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vRuqeCspR7sKMl35oS5SppyzgdpkSGlv-pd3PqUIKttIFcsCW_sQDY7lmfDj5BPKOOxjOBHOAz3p3vh/embed?start=true&loop=false&delayms=3000" frameborder="0" width="960" height="569" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>

# つまずいた点
## API叩いても返ってこない
`axios.post()`  を実行してもリダイレクトを繰り返してエラーを吐く
- 同じ関数を使って別のエンドポイントにリクエストを送ると返ってくる
    - フロント側の問題ではなさそう
- `REST Client` を使うと成功する
    - バック側の問題ではなさそう
    - リクエストの内容もあってそう
- `axios` ではなく `Fetch API` を使っても変化なし
    - ライブラリの問題ではなさそう

### 結果
**CORS** の問題だった
許可してあげると無事に全てのAPIを叩けるように
（なぜ他のは使えたのかは分からない）

### 今後の対策
safari では CORS を無視する設定ができる
chromeで CORSエラーをコンソールに出す設定がある

# よかった点
## ドキュメントを残していた
画面設計 や API設計 などのドキュメントを残しており、
展示会中にアピールすることができた


## デザインを頑張った
最初の一週間を ~~技育展で手を付けれない代わりに~~ **デザインに掛けた**ことにより、
動作に力を入れるチームが多い中、目立つことができた(と思う)


## デモ用のデータを用意していた
バックの完成前にデモ動画を撮るために
フロント側だけで動作するようにしていた

本番当日、たくさんのショートを投稿したところ
`Cloud Storage` の無料枠の制限に達してしまい読み書きが出来なくなった

展示会では デモ用 を併用することで実際の動作を見せることができた

# アピールポイント
## デザイン
デザインに力を入れたのは先ほど同様、アイコンにまでこだわって自作した

## ドキュメント
チーム開発する上でドキュメントを作成しチームで共有した

## 自動再生
動画(ショート) は 1スライド1音声 で構成されていいる.
音声が終わると次のスライドに自動で遷移するのは簡単に実装できたが、
音声の途中で手動で遷移した場合に、停止してから再生するというのに苦労した

## ユーザーを意識した設計
わすれた

# 感想
今まで梶研が参加した HackU では 2連続最優秀賞を取っているという状況の中で参加しました。
とてもプレッシャーの中、開発中には面白くないといった意見などあまり良い評価を得られない状況でした。

そんな中、最後まで完成できただけでなく、最優秀賞を取ることができて嬉しさを超えて安堵感が大きかったです。
もうハッカソンはいい。
