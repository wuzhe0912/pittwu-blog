---
title: CSS 踩坑記錄
date: 2020-05-21 00:28:25
tags:
  - CSS
  - 工作
---
工作上或是進修時，常常會莫名卡在一些奇怪的點，每一次踩到坑都是一次教訓，但為了節省自己的時間，提升開發效率，決定動手把每次踩坑路上的雷點和解法記錄下來，預計一篇筆記會記錄20個雷點與解法。
<!--more-->
## z-index 和 opacity 的覆蓋衝突問題
在 css 中，當元素設定 opacity 時其實具有權重效果，即被自動加入了 z-index，因此倘若有加入 fixed 效果的元素，在經過 opacity 的區域，仍會出現被覆蓋的狀況。

### 解法 1
要解決上述的情況時，需要添加 z-index，但是僅在 fixed 的元素上加入再高的權重，都是無法蓋過 opacity 的元素，需要連添加 opacity 的元素也加入 z-index：
```
.fixed {
  position: fixed;
  z-index: 2;
}

.opacity {
  position: relative;
  z-index: 1;
}
```

### 解法 2
撇除 opacity 不談，其實要達到半透明效果的話，更好的做法應該是採用 rgba 的方式來達成，除了效果相同外，也可以迴避前述的權重問題：
```
.wrap {
  background: rgba($color: #000, $alpha: 0.8);
}
```
- [附上 z-index 說明文章](https://coder-coder.com/z-index-isnt-working/)

## background-size 在 android 手機需注意的細節
一般我在使用圖片作為背景時，偏好使用 background-size 來填滿元素，當然做法有很多，包含 cover、contain、100% 等等，其中需要特別注意 100% 這個寫法。

早先我會寫的比較嚴謹：
```
.wrap {
  background-size: 100% 100%;
}
```
這種寫法是沒有問題的。但是如果寫的比較簡略，如：
```
.wrap {
  background-size: 100%;
}
```
在 ios 手機上或是 chrome 的模擬，都會正常顯示，但是在 android 手機不會正常吃到高的部分(縱軸)，所以這一點需要注意寫法嚴謹度。