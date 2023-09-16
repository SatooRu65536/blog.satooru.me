---
title: "カラーコードボットを作った"
date: 2022-06-12T21:37:15+09:00
weight: 20220612
tags: ["Node.js", "discord BOT", "sqlite"]
categories: ["活動報告", "BOT"]
---


#### 概要
友）カラーコード好きなんだよね。よくネッ友とカラーコード当てゲームしてる  
私）めんどくさくない？BOTにしたら...? ***私がつくろう !!***
<!--more-->
<br>

### カラーコード当てゲーム とは
画像を送ってそのカラーコードを当てるゲーム

![261ac4](./img/017a8b.png)
![261ac4](./img/f83264.png)
![261ac4](./img/ffd876.png)

<br>

### BOT招待・使い方
[BOT招待](https://discord.com/oauth2/authorize?client_id=980406123620356106&permissions=34880&scope=bot)

[ThatsColorCode 使い方](/notyet/)

<br>

### ポイント
#### データの保存
ルームごとのゲームモード等を保存するのに sqlite を使用しています  
<br>

#### 画像生成できない！
canvas を使いたかったが原因不明のエラーにより使えなかった  
→ sharp でpng画像を生成するようにした  
（256×256×256枚の画像生成せずに済んでよかった...）  
<br>

#### 画像データが圧迫する
サーバのglitchには容量制限があるため画像データが邪魔  
→ データベースに生成記録を保存しておき、一定時間で削除されるようになっている  
（ゲーム記録も一定時間で削除されます）

<br>

### 構成
- Node.js 17.10  
- discord.js 12.5.3
- glitch

<br>

### ゲームモード
0. training
    回答したら次の問題がきます
    "fin" を入力すると終了します
1. oneshot
    一人一つカラーコードを入力し、誤差が小さい人が勝ちです
    "check" を入力すると結果が表示されます
2. perfect
    カラーコードをピッタリ入力した人が勝ちです

3. perfectH
    perfect のハードモードです
    ヒントは表示されません

<br>

### コマンド
コマンド
!tcc [gamemode]
    指定したゲームを開始します

!tcc random (num)
    ランダムな色を送信します
    (num) に 1~5 の数字を指定すると複数送信されます

!tcc help
    ヘルプを表示します

#[colorcode]
    入力された色を表示します
    ゲーム中はカラーコードを指定することができます

<br>

### ファイル構成
```
├─ server.js
└─ src
  ├─ finish.js
  ├─ generatecc.js
  ├─ getcc.js
  ├─ help.js
  ├─ oneshot.js
  ├─ perfect.js
  ├─ random.js
  ├─ remove.js
  ├─ training.js
  └─ winner.js
```

<br>

以下の部分でsrcディレクトリ全てのファイルを読み込んでいます
```node:server.js
[server.js:10]

let src = {};
fs.readdir('./src/.', (err, files) => {
    files.forEach(file => {
        base = path.basename(file, path.extname(file));
        src[base] = require('./src/' + file)
    });
});
```

<br>

### カラー画像生成
0~255(10) と 0~ff(16) のランダムな数値を生成

```node
[generatecc.js]

for (let i = 0; i < 3; i++) {
    rgb10[i] = Math.floor(Math.random() * 256);
    rgb16[i] = rgb10[i].toString(16);
}
```

<br>

カラーコード化

```node
[generatecc.js]

let colorcode = ''
for (let i = 0; i < rgb16.length; i++) {
    colorcode += zeroPadding(rgb16[i], 2);
}
```

<br>

画像を生成

```node
[generatecc.js]

const path = `images/${colorcode}.png`;
if (!fs.existsSync(path)) {
    sharp({
        create: {
            width: 100,
            height: 100,
            channels: 3,
            background: { r: rgb10[0], g: rgb10[1], b: rgb10[2] }
        }
    }).toFile(`images/${colorcode}.png`);
}
```

<br>

### GitHub リポジトリ
[SatooRu65536/ThatsColorCode](https://github.com/SatooRu65536/ThatsColorCode)

<br>