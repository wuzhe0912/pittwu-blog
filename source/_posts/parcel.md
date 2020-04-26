---
title: 關於 Parcel：基礎實作
date: 2020-04-21 22:48:33
tags:
  - Parcel
---
Parcel 相較於 webpack 是更新的打包工具，主打兩大賣點，設定簡化 & 打包快速。恰好，最近想幫朋友寫一些簡單的 Landing Page，所以來嘗試 Parcel。
<!--more-->
### Demo
- [Landing Page 連結](https://chihting.netlify.app/)

寫在前面，簡單應用一下 Parcel 這個新的打包工具，實作一個 RWD 的單頁 Landing Page。

第一次嘗試套用 video tag 應用在 background，但效能實在不太好，頁面進入後，從靜態頁面到完成讀取轉為動態影片，需要花不少時間。後續找時間來優化一下效能，看看能不能讓影片讀取速度更快。

### global install
```
yarn global add parcel-bundler

# or

npm install -g parcel-bundler
```

### local install
```
mkdir project-name

cd project

yarn init
```
```
yarn add parcel-bundler --dev

# or

npm install parcel-bundler --save-dev
```
#### package.json scripts setting
```
// --open => auto open browser
{
  "scripts": {
    "dev": "parcel src/index.pug --open",
    "build": "parcel build src/index.pug"
  }
}
```
#### test run
```
yarn dev => 檢查頁面有無正常開啟
```

### SCSS
```
yarn add -D sass

# or

npm install -D sass
```
#### 資料結構
```
- src
  - scss
    - style.scss
    - color.scss
  - js
    - index.js
- index.pug
```
引用方式，將共用 color 或後續的共用函數，透過 @import 載入 style.scss，再把 style.scss 註冊到 index.js。
```
import '../scss/style.scss'
```

### PostCSS 處理前綴詞
```
// mkdir .postcssrc

project
  - src
  - .postcssrc
```
```
// content

{
  "plugins": {
    "autoprefixer": true
  }
}
```
```
// install commend

yarn add -D autoprefixer
```

#### 支援版本
```
// mkdir .browserslistrc

project
  - src
  - .browserslistrc
```
```
// content

last 4 versions
```

### 集成 jQuery
```
// install commend

yarn add jquery
```
```
import $ from 'jquery'
```

### 集成 Bootstrap
```
// install commend

yarn add bootstrap
```
```
// style.scss

@import "~bootstrap/scss/bootstrap";
```

### Clear old data(dist)
```
// plugins

yarn add -D parcel-plugin-clean-dist
```
運行前會清除一次 dist

### 調整生產模式路徑(build)
```
{
  "scripts": {
    "build": "parcel build src/index.pug --public-url ./"
  }
}
```
避免打包出來的路徑出現錯誤