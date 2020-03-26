---
title: Nuxt.js II - 開發準備
date: 2020-03-22 13:33:02
tags: Nuxt.js
---
記錄 Nuxt 專案開發與起步的流程。
<!--more-->
## Nuxt.js 的注意事項
Nuxt => 基底語法以 node.js 為主，因此 node 不支援的語法，即便是 Vue 的語法也不可行。
1. Nuxt 的 `created()` => 不可使用 window、alert、document 等原生屬性。
  - node.js 本身是在 server 端環境運行，而非瀏覽器，自然吃不到瀏覽器的屬性
2. Nuxt => 基底語法以 node.js 為主，因此 node 不支援的語法，即便是 Vue 的語法也不可行。
2. 需優先考慮 node 的執行環境。

### 路徑
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