---
title: Quasar(Vue.js UI Framework)
date: 2020-02-18 15:02:52
tags:
  - Quasar
  - Vue.js
---
Quasar Framework 是支援 Vue.js 的 UI 框架，作者設計出發的概念還蠻宏偉的，兼容全平台，實用性與否還有待評估，這邊先按照作者的教學實作看看。
<!--more-->
## Install
[官網連結](https://quasar.dev/)
```
yarn global add @quasar/cli
# or
npm install -g @quasar/cli
```
### Quasar Cli
```
quasar create project-name

cd project

quasar dev
```
## Install Pug
```
yarn add --dev pug pug-plain-loader
```
### Config
```
// quasar.conf.js

build: {
  extendWebpack (cfg) {
    cfg.module.rules.push({
      test: /\.pug$/,
      loader: 'pug-plain-loader'
    })
  }
}
```
## Install Firebase
```
 yarn add firebase
```
`quasar` 導入 `firebase`
```
quasar new boot firebase

// 新生成檔案在 src/boot/firebase.js
```
設定 `quasar.config.js`
```
return {
  boot: [
    'firebase'
  ],
}
```
### firebase.js
設定內容寫法
```
// Firebase App (the core Firebase SDK) is always required and must be listed first
import * as firebase from 'firebase/app'

// If you enabled Analytics in your project, add the Firebase SDK for Analytics
import 'firebase/analytics'

// Add the Firebase products that you want to use
import 'firebase/auth'
import 'firebase/database'

// 下方貼上 SDK 一大串 script
// 初始化需做調整 Initialize Firebase
const firebaseApp = firebase.initializeApp(firebaseConfig)
const firebaseAuth = firebaseApp.auth()
const firebaseDB = firebaseApp.database()

export { firebaseAuth, firebaseDB }
```
回到 `Vuex` 載入
```
import { firebaseAuth, firebaseDB } from 'boot/firebase'
console.log(1, firebaseAuth)
console.log(2, firebaseDB)
```
## quasar.conf.js
設定 router => History Mode
```
vueRouterMode: 'history'
```
## Error Fixed
資料無法正常送出
```
q-btn 若僅加上 type="submit" 無法正常送出 q-form 內的資料
需在 q-btn 本身再補上一個 @click 事件，才能執行 methods 
```
