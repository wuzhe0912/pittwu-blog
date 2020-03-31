---
title: Nuxt.js II - 框架基礎觀念
date: 2020-03-22 13:33:02
tags: Nuxt.js
---
記錄 Nuxt 專案開發與框架基礎觀念。
<!--more-->
## Nuxt.js 的注意事項
Nuxt => 基底語法以 node.js 為主，因此 node 不支援的語法，在 Nuxt 中也無法運作。
1. Nuxt 的 `created()` => 不可使用 window、alert、document 等原生屬性。
  - node.js 本身是在 server 端環境運行，而非瀏覽器，自然吃不到瀏覽器的屬性
2. Nuxt => 基底語法以 node.js 為主，因此 node 不支援的語法，即便是 Vue 的語法也不可行。
2. 需優先考慮 node 的執行環境。

### Router 路徑
Nuxt 本身已經封裝了許多 Vue-cli 功能，譬如 router 本身在 Vue 需要設置，但在 Nuxt 只要建立好頁面，router 就會自動匹配。

### 方法
- 同 Vue 相同，Nuxt 也是採用 shorthand：
```
methods: {
  formatter () {
  }
}
```

## install scss
在 Nuxt 環境中，`~`、`@`兩者皆通用，都被指向根目錄。但非 nuxt 結構的 js 檔與 nuxt.config.js 則不吃這個規則，必須使用過往的`./`尋找。
- install plugin
```
// pug
yarn add pug pug-plain-loader

// scss
yarn add node-sass sass-loader
```
但是放在靜態資源(static)內的引用規則稍有不同，僅透過`/`，e.g.：
```
link: [
  { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' }
]
```

## nuxt.config.js 設定
- head
就是過往 HTML 頁面的 head 區塊，可以調整頁面的 title，或是添加 meta 來改善 SEO。需要稍微注意 static 靜態資源的飲用路徑略有不同僅`/`。

- css
可以註冊全域 css，方便整個專案調用。

## css
- nuxt 在 css 的環境中，使用 background 載入圖片時，不允許使用 `/`，正確寫法如下：
```
background: url("~assets/images/girl.jpg") no-repeat;
```
這邊也無法用 @ 來載入，唯一只能使用 `~`。

- 預設若圖片小於 1KB，會被 nuxt 轉為 base 64 編碼，使用 data uri 的形式引入。若需要調整上下限，可以在 nuxt.config.js 中的 build 進行調整。
```
build: {
  loaders: {
    imgUrl: { limit: 10000 } // 1000 => 1KB
  }
}
```