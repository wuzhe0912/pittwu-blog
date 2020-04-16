---
title: 實作 Blog 前後台(BootstrapVue + Firebase)
date: 2020-04-13 13:56:40
tags:
  - Vue.js
  - Firebase
  - BootstrapVue
---
沒有使用過 UI Framework 來實作，先嘗試套一個 BootstrapVue 來練習，選擇這個框架的原因，是因為 Bootstrap 依然是蠻多網站的選擇，如果有需要維護同類型的產品，銜接起來相對容易。但是單純的 component 應用就不做記錄了，僅記錄之前未使用過，或是自己少用的用法。
<!--more-->
- [BootstrapVue 官網](https://bootstrap-vue.js.org/docs)

### install
```
yarn add vue bootstrap-vue bootstrap
```

### Use
- 註冊到 main.js
```
import { BootstrapVue, IconsPlugin } from 'bootstrap-vue'

// styles
import 'bootstrap/dist/css/bootstrap.css'
import 'bootstrap-vue/dist/bootstrap-vue.css'

Vue.use(BootstrapVue)
Vue.use(IconsPlugin)
```

### fetch
之前工作採用 axios 套件，但現在 JS 原生語法中已經提供 fetch() 的用法了，而且 fetch 的好處是預設用 promise 包裹，如果有需要就可以直接使用 async/await。而且檢查 can i use，fetch 目前來看兼容性也已經相當高了，所以這個專案開始改用 fetch。
```
fetch(url).then((res) => {
  // 回傳的檔案若為 JSON 格式，需要解析處理
  return res.json()
}).then((result) => {
  console.log(result)
  this.articleData = result.data
}).catch((err) => {
  console.log(err)
})
```