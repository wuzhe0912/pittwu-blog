---
title: JS 基礎知識補充
date: 2020-03-15 08:13:22
tags:
  - Interview
  - JavaScript
---
補強 JS 基礎知識，方便自己隨時複習。
<!--more-->
## JS 變數類型
### value type
```
// 單純的賦值
let a = 10
let b = 20
a = 30
console.log(b) => ? // 20
```
```
let a = 'test' // string
let b = 100 // number
let c = true // boolean
let d = Symbol('d') // ES6 新增

// 另外還有 null、undefined、BigInt
```
### reference type
```
let a = { num: 10 }
let b = a
b.num = 15
console.log(a.age) => ? // 15
```
不同於前者存放於 Stack(棧)，後者存放於 Heap(堆)，存放於堆的變數，看似賦值，但其實是存在電腦中一個位址，即便賦值兩個變數，但其實都是指向同一個位址，也因此就會造成覆蓋。
#### pass by sharing
JS 的屬性，當有`function`時，賦值不影響外部變數，但更新可以改變變數。
```
// 賦值無法影響
let a = { num: 10 }

function changeVal(obj) {
  obj = { num: 50 }
}

changeVal(a)
console.log(a) => ? // { num: 10 }
```
```
// 更新會改變
let a = { num: 10 }

function changeVal(obj) {
  obj.num = 50
}

changeVal(a)
console.log(a) => ? // { num: 50 }
```
- [重新認識 JavaScript: Day 05 JavaScript 是「傳值」或「傳址」？](https://ithelp.ithome.com.tw/articles/10191057)
- [[筆記] 談談 JavaScript 中 by reference 和 by value 的重要觀念](https://pjchender.blogspot.com/2016/03/javascriptby-referenceby-value.html)

### typeof
檢查 value
```
let a = 'test'      typeof(a)   // string
let b = 100         typeof(b)   // number
let c = true        typeof(c)   // boolean
let d = Symbol('d') typeof(d)   // Symbol
let e               typeof(e)   // undefined
```
檢查 function
```
typeof console.log   // 'function'
typeof function(){}  // 'function'
```
陣列、物件、null 都會被判斷為 object
```
typeof null        // object
typeof [1, 2]      // object
typeof { num: 2 }  // object
```
<!-- ### Deep Clone (深拷貝) -->
### 變數計算
```
let a = 10 + 2      // 12
// 如果其中有 string 會變成組合 string
let b = 10 + '1'    // '101'
let c = false + '2' // 'false2'
```
### 判斷檢查
`==`在運算上，會盡可能讓兩側相等，容易出現許多錯誤。
```
10 == '10'         // true
0 == ''            // true
0 == false         // true
false == ''        // true
null == undefined  // true
```
目前基本都是一律使用`===`做判斷，不會使用`==`，但如果為了判斷`null`，可以使用，但我個人是不使用`==`。
```
if (a == null) {}
// 等同於
if (a === null || a === undefined) {}
```