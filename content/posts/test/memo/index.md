---
title: "メモ"
date: 2022-07-21T22:36:32+09:00
description: "さぶかてごりーだよ"
menu:
  sidebar:
    name: memo
    identifier: memo
    weight: 10
    parent: test
keys: ["sub"]
---

# Hi
## ブログの追加方法
```
hugo new posts/2022-07/index.md
```

<br>

## トップ画像の変更方法
`hero.png` または `hero.svg` をフォルダ内に置くだけ
```
test
  ├── hero.png
  └── index.md
```

<br>

## サブカテゴリー
```
test
├── _index.md
├── hero.png
├── sub
│   └── index.md
└── sub2
    └── index.md
```