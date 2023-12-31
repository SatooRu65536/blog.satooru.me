---
title: "毎日の草で猿人が進化するAPI"
date: 2023-11-10T15:23:35+09:00
weight: 20231110
tags: ["api", "express"]
categories: ["活動報告"]
---

## 概要
サークルメンバーがアイデア出ししてる時に思いついたネタです。
草に応じて進化する猿人を作りました。

供養のためにX(Twitter)に投稿したところ、思いの外反響が大きく驚きました。


リポジトリ: [kusa-evolution](https://github.com/SatooRu65536/kusa-evolution)

![kusa evolution](https://kusa-evolution.onrender.com/evolution?username=SatooRu65536)

## 説明
コントリビューションが前日以上であれば進化し、
前日未満であれば退化します。
ただし、コントリビューションが 0 の場合は問答無用で猿人に戻ってしまいます。

#### 1. 猿人
<img src="./assets/enjin.svg" height="120px" />

#### 2. 原人
<img src="./assets/genjin.svg" height="120px" />

#### 3. 旧人
<img src="./assets/kyujin.svg" height="120px" />

#### 4. 新人
<img src="./assets/shinjin.svg" height="120px" />

#### 5. 弥生人
<img src="./assets/yayoijin.svg" height="120px" />

#### 6. 侍
<img src="./assets/samurai.svg" height="120px" />

#### 7. 現代人
<img src="./assets/gendaijin.svg" height="120px" />

#### 8. 未来人(?)
<img src="./assets/miraijin.svg" height="120px" />


## 使い方
USER_NAME には自身のGitHubのユーザー名を入れてください。

```markdown
![kusa evolution](https://kusa-evolution.onrender.com/evolution?username={USER_NAME})
```

### パラメータ
| パラメータ   | 必須   | デフォルト値   | 説明                              |
| :--------- | :---: | :----------: | :-------------------------------- |
| username   |   ○   |      -       | GitHubのユーザー名                  |
| length     |       |      7       | 人間の数                           |
| darkmode   |       |    false     | ダークモード ※                      |
| color      |       |    black     | 色 ※ (cssで指定できる色全て使用可)    |
| bg         |       |    white     | 背景色 ※ (cssで指定できる色全て使用可) |

※ `darkmode` と `color`, `darkmode` と `bg` を同時に指定した場合、
`color` または `bg` が優先されます。

