---
title: 關於 Gulp
date: 2020-04-21 09:28:33
tags:
  - Gulp
---
前端的工具的進化速度，蠻令人感慨的，2017 年甫入行時，Gulp 已經相對式微了，webpack 逐漸站穩舞台。

但即便如此，倘若是做一些輕量頁面或專案，或許 Gulp 仍是不錯的選擇，也或者可能會維護到一些舊專案。因此這邊簡單記錄一些關於 Gulp 的筆記。
<!--more-->
### install
全域環境安裝 gulp
```
yarn global add gulp
```

### init
```
mkdir project-name

cd project-name

yarn init
```

### local install
```
npm install gulp --save
```

### 簡易任務指令
建立兩個測試檔案
```
project
  - source
    - index.html
  - gulpfile.js
```
gulpfile.js 建立任務
```
const gulp = require('gulp')

// gulp.task => gulp 任務
gulp.task('copyHTML', function () {
  /* 
    gulp.src => 資料來源
    /雙星號/*.html => 抓出 source 底下所有 html 檔案
  */
  return gulp.src('./source/**/*.html')
    .pipe(gulp.dest('./public/'))  // 輸出資料到指定資料夾
})
```
終端機運行 gulp copyHTML，建立 public 資料夾，導入 html 文件

### gulp-pug
install command
```
npm install gulp-pug --save
```
gulpfile.js => build task
```
const pug = require('gulp-pug')

gulp.task('pug', function buildHTML() {
  return gulp.src('./source/**/*.pug')
  .pipe(pug({}))
  .pipe(gulp.dest('./public/'))
})
```
run
```
gulp pug

// 可以看到 public 底下建立一個編譯後的 html 文件
```

### gulp-sass
install command
```
npm install gulp-sass --save
```
資料夾結構
```
project
  - source
    - scss
      - style.scss
```
gulpfile.js => build task
```
const sass = require('gulp-sass')

gulp.task('sass', function () {
  return gulp.src('./source/scss/**/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(gulp.dest('./public/css'))
})
```
使用 watch 方法來監聽 scss 的變化進行編譯。
```
gulp.task('watch', function () {
  gulp.watch('./source/scss/**/*.scss', gulp.series('sass'))
})
```

### 4.0 版本的寫法改變
gulp 在 4.0 版本後，調整了部分寫法：
gulp.series  // 按照順序執行
gulp.paralle // 並行執行

合併任務寫法
```
gulp.task('default', gulp.series('pug', 'sass', 'watch'))
```

### gulp-postcss
透過後處理器，來進一步強化 css，譬如 autoprefixer 可以自動加入前綴詞來兼容舊瀏覽器
install commend
```
npm install gulp-postcss autoprefixer --save
```
gulpfile.js => build task
```
const postcss = require('gulp-postcss')
const autoprefixer = require('autoprefixer')

gulp.task('sass', function () {
  const plugins = [
    autoprefixer()
  ]
  return gulp.src('./source/scss/**/*.scss')
    .pipe(sass().on('error', sass.logError))
    .pipe(postcss(plugins))
    .pipe(gulp.dest('./public/css'))
})
```
build .browserslistrc
```
project
  - .browserslistrc
```
內容寫入
```
# Browsers that we support

last 5 version
> 1%
IE 10 # sorry
```

### gulp-load-plugins
優化 gulp 相關套件，節省程式碼
```
npm install gulp-load-plugins --save

const $ = require('gulp-load-plugins')()
```
使用 gulp-load-plugins 後，只要是 gulp- 開頭的套件，都可以無需特別 require，僅只要在對應名稱前面加上 $，範例如下：
```
.pipe($.pug({}))

.pipe($.postcss(plugins))
```
刪除原先的程式碼，促使專案更為精簡
```
// const pug = require('gulp-pug')
// const sass = require('gulp-sass')
// const postcss = require('gulp-postcss')
```

### Babel
install commend
```
npm install --save gulp-sourcemaps gulp-babel gulp-concat @babel/core @babel/preset-env
```
gulpfile.js => build task
```
gulp.task('babel', async () => {
  gulp.src('./source/js/**/*.js')
    // .pipe(sourcemaps.init())
    .pipe($.babel({
      presets: ['@babel/env']
    }))
    .pipe($.concat('all.js'))  // 將所有 js 檔進行合併
    // .pipe(sourcemaps.write('.'))
    .pipe(gulp.dest('./public/js'))
})
```