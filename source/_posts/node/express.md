---
title: Express 框架運用與理解
date: 2020-02-16 10:35:44
tags:
  - Node.js
  - Express
---
Express 算是比較老牌的輕量級 Node.js Web 框架，透過學習使用 Express 來加深自己對 Node 的理解，同時提高對後端的認知。
<!--more-->
## Install
指令：
```
// 初始專案
yarn init

yarn add express
```
## Open Web Server
載入並調用 `Express` 方法：
```
const express = require('express')
const app = express()
```
使用 `get()` 方法，來檢查資料有無傳送到對應頁面
```
app.get('/', (req, res) => {
  res.send('Hello Express!')
})

// watch port
let port = 3000
app.listen(port)
```
### 監聽端口的正式站環境寫法
```
// process.env => 意指環境變數，當完成部署後，端口由 server 提供
const port = process.env.PORT || 3000
app.listen(port)
```
## params & query
這兩者概略來說，就是向後端發出請求時所帶的參數，差別在於，url 上所呈現格式不同：
```
// 按照之前的開發合作的經驗
params => 通常為必填值
query => 選填值

app.get('/user/:name/', (req, res) => {
  let userName = req.params.name
  let limit = req.query.limit
  res.send('<html><head></head><body><h1>'+userName+'</h1><h2>'+limit+'</h2></body></html>')
})
```
## middleware
意指中介層，等於替資料庫做一層防護，當用戶發出請求進入對應頁面，檢查對方是否符合請求邏輯，譬如登入頁可能會檢查一些帳號資訊等等。
```
// 調用 use() 方法做檢查

app.use((req, res, next) => {
  console.log('login')
  next()  // next() 方法是為了當邏輯驗證正確後，能繼續執行下一步驟
})
```
### 驗證檢查
為了避免用戶刻意透過 router 進入頁面，前後端都會進行驗證檢查。此外不存在的資料或是 server 掛了，為了用戶體驗也必須給予對應的狀態提示。
```
// 當用戶隨意亂打參數，使用 use() 方法
// 使其進入我們希望他進入的頁面提示，避免直接回傳 cannot get 訊息

app.get('/', (req, res) => {
  res.send(
    '<html>
      <head></head>
      <body>
        <h1>123</h1>
      </body>
    </html>'
  )
})

app.use((req, res, next) => {
  res.status(404).send('404 is not found')
})
```
`use()` 方法，除了提示找不到頁面，若是程式碼本身有錯，也可以提醒，通常狀態碼為 500。
```
// 這邊使用了一個未定義的函式，除了出現 not defined 的 error
// 也會回傳 500 的狀態碼
app.use((req, res, next) => {
  test()
  next()
})

app.use((err, req, res, next) => {
  console.log(err)
  res.status(500).send('sorry try again')
})
```
### 增加靜態檔案路徑
使用 express 提供的原生方法 `static()`
```
// 建立名為 public 的資料夾作為靜態檔案存放
// 順序上必須放在最前面，方便後面的 router 能正常引用
app.use(express.static('public'))
```
## Template - EJS
### Install