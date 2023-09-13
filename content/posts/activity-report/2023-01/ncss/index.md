---
title: "プログラミング言語を作りました"
date: 2023-01-07T19:50:34+09:00
weight: 20230107
menu:
  sidebar:
    name: "ncss"
    identifier: "2023-01-07T19:50:34+09:00"
    weight: 20230107
    parent: 2023-01
---

## なぜ
今までの会話でこんなことがありました
> A. 使ったことあるプログラミング言語は？  
> B. うーん... **HTML** と **CSS** かな？  

経験者なら違和感を感じると思います  
> **どっちもプログラミング言語じゃなくね？**

厳密に言うと  HTML は マークアップ言語で CSS は スタイルシート言語 です。  
プログラミング言語だ、と言う人もいれば違うと言う人もいます。  

そこで私は思いつきました。  
**「プログラミング言語にすればいい！」**

ということで作りました  
目標は
> CSSをプログラミング言語にする


## はじめに
プログラミング言語を作るには、他のプログラミング言語を使用します
- Ruby は C言語
- Python は C言語, Java...

やはりC言語で実装するのが基本的ぽい、が、めんどくさい！  
そこで私は JavaScript で実装することにしました。

決め手は以下の通りです
- 使い慣れている
- ブラウザで実行できる
- 公開しやすい
- ~~パクれる~~ 参考になるリポジトリがあった

## 自作言語について
言語仕様は [この (GitHub)](https://github.com/ncss-project/ncss#how-to-write-ncss) ようにしました。  
サンプルコードは [こちら (GitHub)](https://github.com/ncss-project/ncss/blob/main/main.ncss) です。

言語名は  
> CSS(カスケーディング・スタイル・シート) ではない

ということで
> NCSS (not CSS)

としました。~~SCSSを意識してますね~~  

工夫点はcssらしくしたことです。(目標だから当然)  
さらに scss らしいあの ~~うるさい~~ ネストを再現できるように、
[グループ](https://github.com/ncss-project/ncss#group) というものを追加しました。
```scss
/* scssらしいうるささの再現 */
#main {
  width: 100vw;

  .container {
    margin: 10px;

    .content {
      font-size: 1.2rem;
      background-color: aqua;
    }
  }
}
```

実装には JavaScriptを使った と書きましたが、制作は TypeScript に切り替えました。
型が無いとあまりにも不便だったからです。型を定義すれば文字列でも選択肢を絞れるから楽でした。  
公開する前に JavaScript にコンパイルしています。


## 使い方
node が入っている場合は↓でインストールできます
```
npm i -g @satooru65536/ncss
```
↓ でncssを実行できます。
```
ncss ./main.ncss
```

ncss を使うためにVScodeの拡張機能も作りました。
- [スニペット](https://marketplace.visualstudio.com/items?itemName=SatooRu65536.ncss-snippets)
- [フォーマッタ](https://marketplace.visualstudio.com/items?itemName=SatooRu65536.ncss-formatter)
- [言語サポート](https://marketplace.visualstudio.com/items?itemName=SatooRu65536.ncss-support)

## 難しかった点
~~こぴぺしたので~~ あまり苦労しませんでしたが、**return** や **berak** の扱いが厄介でした。  
関数の中で同じ関数をなん度も呼び出しているので、どこまで遡って中断すればいいか分からなかったからです。

何言ってるか分からない場合は ↓ を見て察してください。(簡単に説明する方法わからない)
```scss
/* mian.ncss */
#main {
  sub: "";
  content: "main!!";
  return: "";
}

#sub {
  --i: 0;
  .while [--i < 3] {
    .if [--i == 0] {
      return: "sub return";
    }
    transform: --i var(--i) + 1;
  }
  content: "error";
}
```
```js
// ブロック内の処理を順番に実行する関数
function eval_statement_list(/* ... */) {
  for (/* ブロック内の処理を一つずつ */) {
    eal_statement(/* ... */);
  }
}

// 処理を実行する関数
function eval_statement(/* ... */) {
  ...
  else if (/* 関数を呼び出す場合 */) eval_statement_list(/* ... */);
  else if (/* 代入の場合 */) eval_assign(/* ... */);
  else if (/* 変数宣言の場合 */) eval_assign(/* ... */);
  ...
}
```
```js
[処理手順はイメージこんな感じ]

#main 内のブロックを処理 eval_statement_list()
    #sub 内のブロックを処理 eval_statement_list()
    eal_statement(--i: 0;)
    eal_statement(.while)
        .while 内のブロックを処理 eval_statement_list()
        eal_statement(.if)
            .if 内のブロックを処理 eval_statement_list()
            eal_statement(return) // eval_statement_list を何回遡れば...？
            // if ブロック > 中断
            // eal_statement(transform)

        // while ブロック > 中断
        // eal_statement(.if)
        // eal_statement(.if)

    // #sub ブロック > 中断
    // eal_statement(content)

// #main ブロック > 続行
    eal_statement(content)
    eal_statement(return)
// #main ブロック > 中断
// プログラム終了

/* ------------------------------------ */

// 中断と続行の判断基準は？
// => https://github.com/ncss-project/ncss/blob/main/src/evaluator.ts#L70
// called_global_func フラッグがポイント
// 関数のブロックから直接呼び出されているかを判断(if/else/group...のブロックはfalse)
// false なら中断する
// => https://github.com/ncss-project/ncss/blob/main/src/evaluator.ts#L78
```

## おわりに
ちゃんとしたプログラミング言語は難しくても、この程度ならとても簡単でした。  
ぜひ [ソースコード](https://github.com/ncss-project/ncss) を解読してみてください
