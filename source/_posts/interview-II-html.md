---
title: 常見面試問題：HTML
date: 2019-12-05 17:36:59
tags:
  - Interview
  - HTML
---
HTML 面試問題彙整。
<!--more-->
### HTML5 特色
廣義而言，新增了相當多不同領域功能的 tag，語意化，諸如 header、main、section 之類的語易化標籤，善用這些標籤能對網站 SEO 能更加友善。多媒體，隨著 HTML5 開始支援播放影片音樂，也成了壓垮 flash 的最後一根稻草。Canvas 除了讓網頁直接繪出圖形，也讓許多遊戲產業獲得更好的發展。其他則還有支援移動裝置的 touch 事件、地理資訊紀錄以及雙向溝通的 websocket(但存有安全性疑慮)。

### `doctype` 做什麼用的？
```
用來定義網頁編寫的 HTML 採用什麼版本。
```

### 有用過 HTML 樣板語言（template languages）嗎？
目前使用 `pug`，除了減少閉合標籤，也能讓 template 更為簡潔。

### 性能優化
- 前端效能優化可以怎麼做？
壓縮文件大小，除了 webpack 本身的打包壓縮，圖片可以透過 tinypng 來壓縮圖片大小，另外顏色豐富的圖片或較大的圖片，優先考慮 jpg。如果要針對性地去優化，可能要使用開發工具中的 performance 來檢查。

