---
title: Webpack 設定與理解
date: 2020-04-15 16:56:06
tags:
  - Webpack
---
雖然到 vue-cli 3.0 之後，其實 vue 官方已經將 webpack 進行封裝，希望開發者專注在開發而非設定上，但還是需要理解基本的設定，以及細節的理解。
<!--more-->
### 模組化
現代開發專案的複雜度，容易面臨維護上的不易。為了更有效的管理專案開發，模塊化、組件化的思想蔚為風潮，而在 Node.js 中模塊化的思想自然也是重中之重。require 引入 & module.exports 導出，被頻繁用來抽換模塊。但對瀏覽器來說，原生不支援這種 CommonJS 標準，所以我們仰賴 webpack 這種工具將程式碼進行打包(類似早期的 browserify)。同時 webpack 已經預先將 babel 此類工具封裝好，在處理 ES6 之類的語法時，會自動進行轉換，可以讓我們省下不少工。

### 使用 webpack 的理由？
1. 透過 webpack 幫助我們針對檔案進行壓縮(uglify)和輕量化(minify)。
2. 圖片或 CSS 都能以資源的方式引入。
3. 如果想要安裝 npm 上的第三方套件，會更為輕鬆容易。

### 常用的 loader 有哪些？
- 樣式類：style-loader、css-loader、sass-loader
- 自動編譯：babel-loader、ts-loader
- 文件相關：raw-loader、file-loader、url-loader
- 自動測試或書寫規範：eslint-loader、jshint-loader、mocha-loader
e.g.：
```
// sass-loader => 將 sass 轉為 css
// css-loader => 解析並處理 @import || url() 的用法
// style-loader => 協助建立 style 標籤，並將 css 檔案載入到 html

module.exports = {
  module: {
    rules: [
        {
          test: /\.scss$/,
          use:[
              {loader:'style-loader'},
              {loader:'css-loader',options:{sourceMap:true,modules:true}},
              {loader:'sass-loader',options:{sourceMap:true}}
          ],
          exclude:/node_modules/
      }
    ]
  }
}
```

### 有使用過哪些插件？
許多常用的 plugins 都已被 cli 腳手架封裝進去，譬如自動壓縮程式碼 UglifyJsPlugin，即時熱更新 HotModuleReplacementPlugin。

#### DefinePlugin
因為可以在編譯改變全域的變數，因此可以修改設定來達到生產環境和開發環境的差異，但我目前還沒實際設定過。

#### IgnorePlugin
可以用於忽略特定的模塊或文件，例如特定較大的 library，可能需要在打包時進行忽略，進而縮減專案體積。

<!-- ### module chunk bundle 差別
- module
  - 在 webpack 當中，一切都是模組，module 等同於源碼文件 -->

### 所以為何要使用 build？
承前面的問題，透過這一類的打包工具(gulp || webpack)，來進行打包優化，並將程式碼轉為瀏覽器可以理解並支援的語法，如此才能在瀏覽器上運行。

### vue.config.js
#### publicPath
只要專案內有設定二級 router，就必須設定 publicPath，但除非你希望調整 url 出現的路徑，否則一般都使用 `/`：
```
// 寫在 module.exports 內任一處即可

module.exports = {
  publicPath: '/'
}
```

#### outputDir
設定 Vue 打包後的資料，要生成到那個資料夾內，可任意改成你想要的資料夾名稱，預設為 dist：
```
module.exports = {
  publicPath: '/',
  outputDir: 'dist'
}
```

#### indexPath
指定打包後生成的 html 檔案名稱，無需特別變動。

#### filenameHashing
瀏覽器基本上都會有緩存的問題，所以如果都是同一個檔案名的狀況下，譬如都是 app.js，那假定我更新一個版本，結果瀏覽器緩存，導致它拉到 app.js 是上一個版本，那就尷尬了。所以透過 hash 的方式，來生成獨一無二的檔案名，避免被緩存卡住，預設為 true，不需要調整。

#### lintOnSave
檢查 code 有沒有符合 eslint 規範，預設開啟，但也不需要特別調整，畢竟現在開發都會遵循 eslint，至於採用哪種風格，則是依照團隊 coding style。

#### chainWebpack
根據自己的需求，進行客製化調整，譬如希望修改網頁上方標題：
```
chainWebpack: (config) => {
  config
    .plugin('html')
    .tap((args) => {
      args[0].title = 'Tiger Coding Blog'
      return args
    })
}
```