---
title: Vue-Cli 初始化專案操作記錄
date: 2020-02-12 20:13:27
tags: Vue.js
---
前端框架版本迭代太快，更新過程中安裝的版本又容易出現錯誤，單靠記憶不可行，只好記錄自己的操作步驟，隨時更新方便檢查。
<!--more-->
## 全域環境調整(vue-cli 4.0 以上版本)
### remove old version
```
npm uninstall vue-cli -g

# or

yarn global remove vue-cli
```
### install new version
```
npm install -g @vue/cli

# or

yarn global add @vue/cli
```
### check version
```
vue --version

# or

vue -V
```
## fixed yarn
### clean yarn cache
yarn 更新至 v1.19.0 後，一度在終端機一直跳提醒，google 到的解法是清除 cache。
但目前已是 v1.22.0 已無目前問題。
```
yarn update v1.19.0 後，需清除一次 cache，指令如下：

yarn cache clean
```
## build project
### version not match error
```
若出現vue的版本為 2.6.9，但套件採用 2.6.10 的 error

需升級vue的版本，指令如下：

yarn global add vue@2.6.11
```
## create
```
vue create project-name
```
### 選擇需要的項目
官方預設提供的 css 預處理器，存在一些配置上的錯誤，這邊不做使用
僅選擇 Router & Vuex(單元測試和 PWA 尚未掌握)
Router 採用 History Mode
### 進入項目並啟動
```
cd project
yarn server
```
## install Plugins
### API
```
yarn add axios vue-axios
```
### 驗證
[vuelidate](https://github.com/vuelidate/vuelidate)
```
yarn add vuelidate
```
### 拖曳
[vuedraggable](https://github.com/SortableJS/Vue.Draggable)
```
yarn add vuedraggable
```
### 預處理器
```
// pug
yarn add vue-cli-plugin-pug --dev

// scss
yarn add vue-cli-plugin-pug sass sass-loader --dev

```
## 專案結構
```
assets
components
views
```
### scss
```
assets
  - scss
    - color.scss => 基礎共用色碼
    - mixin.scss => 基礎共用函式
    - share.scss => @import 上面兩個基礎共用scss，建立共用參數
    - style.scss => @import share.scss => 全域 scss，權重最重，在此處 reset css

main.js
  - import 'scss/style.scss'
```
#### scss => vue.config.js
```
// 調整相對路徑，方便 component 引入樣式
chainWebpack: config => {
  config.resolve.alias
    .set('scss', resolve('src/assets/scss'))
}
```
### router
拆分為 index.js 和 map.js
當頁面數過多時，將 routes 抽離至 map.js，再載入到 index.js
### webpack 設定
建立 vue.config.js
```
const path = require('path')
function resolve (dir) {
  return path.join(__dirname, dir)
}

module.exports = {
  publicPath: '',
  // 調整本地端口
  devServer: {
    host: 'localhost',
    port: 8081,
    // 設置代理 => 解決跨域問題(調用後端API接口時通常不是同一個域名)
    proxy: {
      '/api': {
        target: 'http://localhost:8081',
        // websocket 縮寫 => ws
        ws: false,
        // 避免在訪問網址時，自動將原點移除
        changeOrigin: false
      }
    }
  },
  chainWebpack: (config) => {
    config.resolve.alias
      .set('@', resolve('src'))
  }
}
```