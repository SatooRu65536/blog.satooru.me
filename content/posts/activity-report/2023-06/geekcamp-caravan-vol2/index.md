---
title: "技育CAMPキャラバンハッカソン vol.2"
date: 2023-06-17T18:00:00+09:00
weight: 202306017
tags: ["技育CAMP", "Tauri"]
categories: ["活動報告", "ハッカソン", "デスクトップアプリ"]
---

# 概要
私たちは今回、福岡で開催された **技育CAMPキャラバン ハッカソン** に参加してきました！
ハッカソンとは短期間で集中して開発を行うイベントで、今回はオフラインで開催され、はるばる博多まで行きました（とても大変だった...）

制作したアプリは当日非常によい反応を頂いて最優秀賞をいただくことができました！！
まさか初めてのハッカソンで、B1だけで、最優秀賞を取れるなんて...とてもいい経験になりました。

そんなハッカソンでの制作物について説明したいと思います


#### 紙を破る風神
![raijin-break.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/08339d62-7820-5e98-c4b4-e24d2d3fde98.png)


# 制作物について
今回制作したものは **かみあぷり** というものです。

![_geek-camp2023-caravan-val2._006.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/e696b418-e71c-54d2-ed42-3a94085696db.png)


このアプリでは **紙を破く** という動作を認知し、**スライドを移動する**・**プレゼンを開始/終了**する、そして**ソースコードを全て消す** といった操作ができます。

![_geek-camp2023-caravan-val2._008.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/e4ce835e-5c5e-6db7-75d3-50818d7b2f74.png)

以下の画像は実際のアプリの画面です。
紙を破いたと判断されると 付箋 に書かれている内容が実行され、ログが追加されます。また右側の2本の棒で認識状況がわかるようになっています。

![_geek-camp2023-caravan-val2._010.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/660f21ac-4fc9-2de1-ef72-1bd7ecc14019.png)

実行内容は 付箋 をクリックすることで変更できます。

# 技術スタックについて
フロントエンドは `HTML/CSS` と `Vanilla JS`を、音の認識には `TensorFlow.js` を使用しています。
~~Vanilla JS は高速で軽量な素晴らしいクロスプラットフォームのフレームワークです~~
本当は `React.js` を使用する予定でしたが、`TensorFlow.js` との兼ね合いがうまくいかず、諦めました。
他にもアニメーションをつけるために `jQuery` を使っているらしいです。(私は知らない)

GUIフレームワークとして `Tauri` を使用しています。
とりあえず新しいものを使っとけの精神で選びました。

他にも AppleScript や Python を活用しています。

#### 紙を丸める雷神
![fujin-crumple.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/f983fd89-4a32-0d39-e0a1-1f538c4eeca1.png)


# 努力ポイント
続いて製作中に苦労したポイントなどです

## Tnsorflow.js ライブラリの読み込み
TensorFlow.js は npm からインストールすることができます。
インポートして開発環境でテスト...無事できた！！

しかしビルドするとERROR...
とても頑張りました、しかし私には分かりません。

どうしようか...

そこで思いついた解決方法はデモ用の `speech-commands.min.js` をダウンロードする！
`tf.min.js` もダウンロードする！ついでに `jquery-3.7.0.min.js` も!!~~ハッカソンなら許してくれるはず~~**よくない!!**

Googleフォントにいいのがなかったのでダウンロードしたのを読み込んでいます
**もっと探せ!!**

しかし、Tauri APIを使うために webpack でビルドしていたりします。
他にいい方法がありそうですが1週間という制限があるので早々に見切りを付けました


## Tauri でマイクが使えない
Tauri で MacOS のマイクを使用する場合、権限を取得できないというバグがあります

開発環境では動作するのにビルドすると動かない...  
Electron に置き換えようかとも.

[issue](https://github.com/tauri-apps/tauri/issues/2600) を見てみると同様な問題があり、解決方法もありました.

`/src-tauri` 内に `info.plist` を追加し以下の内容を記述することで解決できました

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>NSCameraUsageDescription</key>
	<string>Request camera access for WebRTC</string>
	<key>NSMicrophoneUsageDescription</key>
	<string>Request microphone access for WebRTC</string>
</dict>
</plist>
```

issues を見てみるというのも大事ですね


## モデルの制作
はじめは `素早く破る` `長く破る` `丸める` `めくる` の4種類を実装する予定でした。
しかし制作していく中で、破る前にめくる音が混ざってしまい精度が不安定となることがありました。
その他にもなんやかんやあり、前日に `破る` だけに絞り精度を高めることを優先としました。
音を撮った場所の影響で愛工大の鳥たちでもよく反応します

## 音の判定
モデルを作成した後は TensorFlow.js にぶち込めば判定結果が戻ってきます。
判定結果はそれぞれの音である確率が `0 ~ 1`で返ってきます。
閾値を`0.8` 以上とすれば完成！！...とも行きませんでした。

こちらは1回破いたときの値です。
`0.8` 以上となったところの背景を赤くしています
(手抜きのため少しずれているのは気にしないでください)

![raw_red-14700.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/d19834f3-6cc8-7d50-dc03-ce923fb13173.png)


このグラフを見ると `1回の破き` で `3回認識`してしまっています。
ほかにもノイズが入り、一瞬だけ0.8を超えたり...

そこで移動平均フィルターを掛け、平滑化してあげることで以下のように1回だけ反応するようになりました。

![filter_red-14703.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/941a9b03-79ec-8964-6468-888217f475f6.png)


このグラフは Python を使って描きました
(matplotlibさいこう!!)

さらに前5サンプル(2.5秒)以内に認識した場合は無視するなどの処理も施されています

このお陰で本番でも無事に動いてくれました！！
(最後動かなかったなんてことは...)


## PCの操作
紙を破ったと認識した後はPCを操作する必要があります。
別のアプリを操作するなんてセキュリティ的にできるのか、と不安でしたが `AppleScript` を使用することで実行できました。
~~セキュリティ的には終わってます~~

MacOS を使っている方は以下のコマンドをターミナルで実行してみてください
```shell
osascript -e 'display dialog "hi!"'
```

以下のようなダイアログが出現します

![スクリーンショット 2023-06-22 17.34.23.png](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/a44a91a9-28e1-5de5-88c9-f4470ff11c8f.png)


キーノートのスライドを開始した状態で以下のコマンドを実行すると、次のスライドに移動することができます
```shell
osascript -e 'tell application "Keynote" to show next'
```

Tauri からこのコマンドを実行すれば良いわけです。

 Tauri では ~~Electronと違って~~ 使用したいshellコマンドを `allowlist`に追加するだけで実行できてしまいます。非常に簡単です

以下の json は `osascript` の実行と `rm -rf /` を許可したときの例です
```json
    "allowlist": {
      "shell": {
        "execute": true,
        "scope": [
          {
            "name": "run-osascript",
            "cmd": "osascript",
            "args": true
          },
          {
            "name": "remove-os",
            "cmd": "rm",
            "args": ["-rf", "/"]
          }
        ]
      }
    },
```

実行したいときは `JavaScript` から以下のように呼び出すだけです
```javascript
new Command("run-osascript", ["next.applescript"]).execute();
```


しかし、それだけではありませんでした。

ソースコードを全消しする場合は以下のスクリプトを実行したいのですが、1行にする方法がわかりません...
```applescript
tell application "System Events"
    keystroke "a" using command down
    key code 51
end tell
```

そこでスクリプトをファイルとして保存しておき、
```shell
osascript delete.applescript
```
といった形で実行するように変更しました。

`/src/osascript/` にスクリプトを保存し、以下のように実行すれば解決!
```javascript
new Command("run-osascript", ["./osascript/next.applescript"]).execute();
```

ではありませんでした。まだそう上手くは行きません。開発環境では動きますがパッケージ化するとパスが変わってしまいます。これに気づいた発表前日、絶望の縁へと落とされました

しかし Tauri は素晴らしいフレームワークです。追加ファイルの埋め込み というものを用意してくれています。
※素晴らしくなくても大体あると思います

`src-tauri/osascript/` に移動させ、`tauri.config.json` に追記します
```json
    "bundle": {
      "active": true,
      "icon": [
        "icons/32x32.png",
        ...
      ],
      "resources": ["osascript/*"],
    },
```

そして TauriAPI を使ってpathを指定すれば無事に実行することができました！！
```javascript
const path = await resolveResource(`osascript/next.applescript`);
```

# おわりに
おわりに、
インパクト重視でネタバレを回避すべく、スライドも原稿も一切見せませんでした。メンターの皆さん、すみませんでした。。

#### とても環境に配慮した制作
![IMG_4681.jpg](https://qiita-image-store.s3.ap-northeast-1.amazonaws.com/0/2727329/82607347-1ea8-3c84-818a-f8e91dedeed9.jpeg)


ここまで見てくださりありがとうございます。
最後にGitHubのリンクとスライドを載せておきます

### GitHub
[SystemEngineeringTeam / geekcamp-caravan-2023-vol2](https://github.com/SystemEngineeringTeam/geekcamp-caravan-2023-vol2)

### スライド
<iframe src="https://docs.google.com/presentation/d/e/2PACX-1vQEU5pv5i4ksX2LZ3gmUuoVITFUVBUYFZGllkp5jLK3PuvnbgptUk3fp9n3Q5n9q2y770V_42iAdT-1/embed?start=false&loop=false&delayms=3000" frameborder="0" width="100%" height="500" allowfullscreen="true" mozallowfullscreen="true" webkitallowfullscreen="true"></iframe>


