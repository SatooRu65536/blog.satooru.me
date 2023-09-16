---
title: "梶研 05/10 - 05/16"
date: 2023-05-16T20:00:00+09:00
weight: 20230516
tags: ["Unity", "歩行軌跡"]
categories: ["活動報告", "梶研"]
---


# スケジュール
- [x] Unityでアニメーション


# 進捗
## Unityでアニメーション
### c#スクリプトの注意点
ファイル名とクラス名を一致させなければならない  
(2時間無駄にした...)


### オブジェクトの移動(キーボード)
<iframe width="560" height="315" src="https://www.youtube.com/embed/TvVuvuxcTOc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

> this.transform.position = new Vector3(x, y,z);

単純な移動はこれだけ

### 軌跡に沿ったオブジェクトの移動
<iframe width="560" height="315" src="https://www.youtube.com/embed/-4mBXV7yWng" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

```cs
void Start() {
    ...
    while (!reader.EndOfStream) {
        string[] line = reader.ReadLine().Split(',');
        float x = float.Parse(line[1]);
        float y = float.Parse(line[2]);
        float z = float.Parse(line[3]);
        positions.Add(new Vector3(x, z, y));
    }
    ...
}

void Update() {
    // アニメーションを実行する
    timer += Time.deltaTime * 4;
    if (timer >= 1f) {
        Debug.Log(timer);
        currentIndex++;
        if (currentIndex >= positions.Count) currentIndex = 0;

        transform.position = positions[currentIndex];
        timer = 0;
    }
}
```

GIFアニメみたい  
=> 動作をなめらかにした

１歩進むごとの時間間隔が一定となっている  
=> 測定時に合わせたい


### 実際の時間間隔に合わせたもの
<iframe width="560" height="315" src="https://www.youtube.com/embed/K2JixDncg1s" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

実際の4倍速にしてある

次に移るまでの間隔を前の位置からの時間差に変更
```cs
void Start() {
    ...
    while (!reader.EndOfStream) {
        string[] line = reader.ReadLine().Split(',');
        float x = float.Parse(line[1]);
        float y = float.Parse(line[2]);
        float z = float.Parse(line[3]);
+       positions.Add(new Vector3(x, z, y));
    }
    ...
}

void Update() {
    // アニメーションを実行する
    timer += Time.deltaTime * 4;

+   float diff_time;
+   if (currentIndex == 0) diff_time = 0;
+   else diff_time = times[currentIndex] - times[currentIndex - 1];

+   if (timer >= diff_time) {
        currentIndex++;
        if (currentIndex >= positions.Count) currentIndex = 0;

        transform.position = positions[currentIndex];
        timer = 0;
    }
}
```

### 滑らかにしたもの
<iframe width="560" height="315" src="https://www.youtube.com/embed/TBRMeLTtI4c" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


自分で実装するのは面倒だからアセット `DG.Tweening` を使用した
```cs
+ using DG.Tweening;

void Update() {
    // アニメーションを実行する
    timer += Time.deltaTime * 4;

    float diff_time;
    if (currentIndex == 0) diff_time = 0;
    else diff_time = times[currentIndex] - times[currentIndex - 1];

    if (timer >= diff_time) {
        currentIndex++;
        if (currentIndex >= positions.Count) currentIndex = 0;

-       transform.position = positions[currentIndex];
+       this.transform.DOMove(positions[currentIndex], diff_time);
        timer = 0;
    }
}
```


# メモ
向いている方向も

unityはあくまでもCGではなく、成果を伝えるもの

TODO:  
- 直角, 並行から歩行軌跡を綺麗に
- 階段とエレベーターの気圧のグラフの差をみてみる
