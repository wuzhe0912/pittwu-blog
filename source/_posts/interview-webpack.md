---
title: 面試問題：Webpack
date: 2019-06-09 13:46:58
tags:
  - Interview
  - Webpack
---
Webpack 是目前近兩年最熱門的打包工具，功能強大與客製化程度高，與之相對的，在設定上有很多眉角。另一方面，因為工作上使用 Vue cli 已經預設好相當多功能，反而對 Webpack 的認識不足。之後，需要練習使用純 webpack 來搭建環境而不使用框架。
<!--more-->
### webpack 中 babel 如何設定？
- plugins install --dev
```
@babel/cli
@babel/core
@babel/preset-env
```
基本設定寫在 .babelrc 的檔案中：
```
{
  "presets": [
    [
      "@babel/preset-env"
    ]
  ],
  "plugins": []
}
```
@babel/preset-env，通常會用來放一些常用的語法解析，預設 babel 官方已經有提供 ES6 語法解析。babel 本身主要專注在解析語法，但新的 API 會被忽略，因此需要 polyfill 來輔助。

- babel-polyfill
  ```
  @babel/polyfill
  ```
  babel-polyfill 就是延伸早期的 polyfill 方案，再結合 core-js 和 regenerator 形成強大的集合，幫助開發者將所有的新的語法打上補丁，讓他們可以在低版本的瀏覽器正常運作。但 babel 官方 7.4 版本後，建議直接使用 core-js 和 regenerator。但因為 babel-polyfill 本身體積蠻大的，畢竟是集合解析所有語法，所以需要在 .babelrc 調整設定：
  ```
  {
    "presets": [
      [
        "@babel/preset-env",
        {
          "useBuiltIns": "usage",
          "corejs": 3
        }
      ]
    ],
    "plugins": []
  }
  ```
  usage 即可以只引入專案中有用到的語法，3 則是當前 core-js 版本號。

- babel-runtime
  ```
  @babel/runtime

  // dev
  @babel/plugin-transform-runtime
  ```
  babel-polyfill 本身會有污染全域的問題，所以需要透過 babel-runtime 來處理，安裝完插件後，設定如下(直接照官方預設設定)：
  ```
  {
    "presets": [
      [
        "@babel/preset-env",
        {
          "useBuiltIns": "usage",
          "corejs": 3
        }
      ]
    ],
    "plugins": [
      [
        "@babel/plugin-transform-runtime",
        {
          "absoluteRuntime: false,
          "corejs": 3,
          "helpers": true,
          "regenerator": true,
          "useESModules": false
        }
      ]
    ]
  }
  ```
