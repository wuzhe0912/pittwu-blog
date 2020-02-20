---
title: Quasar(Vue.js Framework)
date: 2020-02-18 15:02:52
tags:
  - Quasar
  - Vue.js
---
Quasar Framework 是支援 Vue.js 的 UI 框架，作者設計出發的概念還蠻宏偉的，兼容全平台，實用性與否還有待評估，這邊先按照作者的教學實作看看。
<!--more-->
## Imstall
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
### Install Pug
```
yarn add --dev pug pug-plain-loader
```
#### Config
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
### Install Firebase
```
 yarn add firebase
```
## Error Fixed
資料無法正常送出
```
q-btn 若僅加上 type="submit" 無法正常送出 q-form 內的資料
需在 q-btn 本身再補上一個 @click 事件，才能執行 methods 
```
