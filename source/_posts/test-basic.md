---
title: 前端自動化測試基礎知識
date: 2020-03-18 13:26:33
tags:
  - 自動化測試
  - Jest 
---
之前工作的團隊是完全不碰這塊的，所以本身沒有摸過，但考慮到目前整體趨勢上，不少公司的前端正在導入測試，還是記錄一些筆記，方便未來能夠上手。
<!--more-->
## 目的
e.g.
許多舊專案的維護，改動不易，且重構成本高昂，且在重構的過程中，可能面臨需求改動，或是重構過中忽略一些歷史因素，導致功能不足前功盡棄。為了降低重構成本，在重構前預先準備測試，這樣在階段性重構的過程，這些測試皆會被調用，若測試沒過，代表重構的程式碼存在問題，第一時間先進行修復，避免出現重構完才發現重構程式存在瑕疵。
### 範例
結構
```
- index.html
- math.js
- math.test.js
```
我將 math.js 寫好兩個計算函數並引入 index.html，如下：
```
function add (a, b) {
  return a + b
}

function minus (a, b) {
  // 這邊我故意寫錯，將減法寫成除法
  return a / b
}
```
同時準備測試代碼在 `math.test.js`，當然這邊的結構是無法直接執行測試，所以我們將測試程式丟到瀏覽器上的`console`測試時，在 run 的過程中因為函數本身有錯誤，這時就會進入 throw，提醒我哪邊發生錯誤了，可以回頭去檢視。
```
// 預先將結果寫出來
var result = add(2, 4)
var expected = 6

if (result !== 6) {
  // throw 拋出例外狀況
  throw Error(
    `2 + 6 應該等於 ${expected}，但結果卻是 ${result}，請檢查add()`
  )
}

var result = minus(10, 5)
var expected = 5

if (result !== 5) {
  throw Error(
    `10 - 5 應該等於 ${expected}，但結果卻是 ${result}，請檢查minus()`
  )
}
```
如此一來，未來如果進行擴充新函數時，而我不小心手殘改到舊的函數時，在 run 測試的時候，我也可以獲得提醒，避免發生不必要的 bug。
### 封裝
除了上述寫法，也可以將測試的函數合併在一起。
```
function expect (result) {
  console.log(result)
  return {
    toBe: function(actual) {
      console.log(actual)
      if (result !== actual) {
        throw new Error(
          `程式碼執行結果和預期結果不同，預期是${actual}，結果卻是${result}`
        )
      }
    }
  }
}

// 測試描述，方便測試後，更好理解哪一部分出現錯誤
function testDescription(desc, params) {
  try {
    params()
    console.log(`${desc} 已通過測試`)
  }
  catch(err) {
    console.log(`${desc} 未通過測試 ${err}`)
  }
}

testDescription('測試加法函數', () => {
  expect(add(2, 4)).toBe(6)
})

testDescription('測試減法函數', () => {
  expect(minus(10, 5)).toBe(5)
})
```
假設我故意將`minus()`寫成+法，那拿到的 result(函數計算的結果)和 actual(預期的結果)將不會相等，則會拋出 throw。
## Jest
- 速度快
  - a、b兩者互不關聯的函數，若僅改a，但b未改動，則b不做測試，節省測試時間。
- 多項目並行
  - 若前後台專案，使用不同框架或是一方使用Vue，另一方使用Node，依然可以並行測試。

### install
```
cd project
yarn init
```
```
// 測試僅需要運行在開發環境
yarn add jest --dev
```
### use
在`Jest`中，已經提供了`test()`和`expect()`，直接使用即可。
- 導出，在`math.js`中使用`module.exports`將函數導出。
```
function add (a, b) {
  return a + b
}

function minus (a, b) {
  return a - b
}

module.exports = {
  add,
  minus
}
```
- 引入，在`math.test.js`中將導出的函數引入測試。
```
const math = require('./math.js')
const { add, minus } = math

test('測試加法函數', () => {
  expect(add(2, 4)).toBe(6)
})

test('測試減法函數', () => {
  expect(minus(10, 5)).toBe(5)
})
```
- `package.json`腳本加入運行`Jest`指令
```
// 這個指令執行後，會去尋找目錄下，以 test.js 結尾的文件，並加以運行
"scripts": {
  "test": "jest"
}
```
接著在終端機運行`yarn run test`，就可以看到測試的過程。
`Jest`的主要功能在於單元測試與集成測試，但這兩者本質上就是單一模組測試和多模組測試。所以為了配合`Jest`，必須將既有的函數透過`module.exports`的方式轉為模組化後，才能讓`Jest`調用測試。
- 測試環境和瀏覽器環境
一般來說，在沒有`cli`之類的工具下，瀏覽器無法自動編譯`module`，但對測試環境來說需要`module`，所以需要透過`try catch`包裹，避免出現紅字錯誤。不過實務上，我們目前開發大多已在腳手架工具內，自動會協助處理編譯，所以其實是不需要`try catch`包裹。
```
try {
  console.log('test')
  module.exports = {
    add,
    minus
  }
}
catch (err) {
  console.log(err)
}
```

### 設定
- 指令
```
yarn jest --init
```
這道指令的目的，在於啟用`node_modules`下的`Jest`進行初始化。執行指令後，接下來會詢問測試環境是`Node`還是瀏覽器，是否生成覆蓋率報告，測試結束後是否自動清除模擬。這邊選瀏覽器環境，其他則都選y，最後目錄下會生成`jest.config.js`的檔案。
- jest.config.js
在設定檔中，可以看到下面這一行，意思是生成覆蓋率報告時，會自動存放於`coverage`的資料夾內。
```
coverageDirectory: "coverage"
```
同時因為剛剛選擇的是瀏覽器環境，所以預設會註釋`browser`
```
// browser: false
```