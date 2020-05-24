---
title: Node 基礎知識
date: 2020-01-27 23:35:59
tags: Node.js
---
記錄自己學習 Node.js 基礎API用法。
<!--more-->
## require、module exports
`require`語法可以載入檔案，反之`module.exports`則能導出內容。寫法如下：
app.js
```
let content = require('./app2')

console.log(content)
```
app2.js
```
let data = 'node-test'

module.exports = data
```
印出`'node-test'`字串。

除此之外也能放入物件格式：
```
module.exports = {
  title: data,
  subTitle: 'title-test',
}
```
印出對應的物件。
### 相對冷門寫法
一般導出寫法多採用`module.exports`，但單純使用`exports`也能導出。
```
exports.subContent = 123

// 這個寫法等於建立一個物件，如下：
module.exports = {
    subContent: 123
}

//除了物件外，也能調用函式
exports.call = (() => {
  return 'call!'
})
```
## http API
### createServer
Node 原生 http API 用法
```
const http = require('http');

// request 使用者發出請求，常縮寫為 req
// response 回傳結果，常縮寫為 res

http.createServer((req, res) => {
  // 表頭內容
  res.writeHead(200, { "Content-Type": "text/plain" })
  // 回傳結果內容
  res.write('Hello Node!')
  // 結束
  res.end()
}).listen(8081) // 監聽 port

```
## 查詢路徑或檔案名稱
`__dirname`語法，可以查詢到當前檔案所在的路徑。
`__filename`語法，則能檢查當前檔案名稱。
## path
Node 原生 path API 用法
```
const path = require('path')

// 抓目錄路徑，回傳 /Path/subNode
console.log(path.dirname('/Path/subNode/index.js'))

// 路徑合併
console.log(path.join(__dirname, '/Path'))

// 抓取檔案名稱，回傳 index.js
console.log(path.basename('/Path/subNode/index.js'))

// 抓副檔名，回傳 .js
console.log(path.extname('/Path/subNode/index.js'))

// 分析路徑
console.log(path.parse('/Path/subNode/index.js'))
```