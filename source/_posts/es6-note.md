---
title: ES6：筆記整理
date: 2020-02-09 22:05:23
tags: ES6
---
整理散落各方的 ES6 筆記，方便自己日後查詢。
<!--more-->
## 變數宣告
傳統變數宣告採用 `var` 的方式，但容易存在污染環境的問題。因此在 ES6 的語法中，新增兩個變數宣告方式 `let、const`。
### const
使用 const 定義常數，意味著這個變數不作變動，且要求一定要賦值。
```
const a = 1
```
### let
和傳統的`var`相似，側重解決過往的 scope(執行環境)問題。使用`let`宣告變數時，僅會作用在當前區域，且可以選擇賦值或不賦值，通常以 function 本身為分界。
- var
```
if (true) {
    var b = 10
}

console.log(b) // 印出 10
```
- let
```
if (true) {
    let c = 20
}

console.log(c) // error，c is not defined
```
## 樣板字面值(模板字符串) Template literals
過往`ES5`寫法如下：
```
var name = 'Pitt'
console.log("My name is " + name)

// 印出 My name is Pitt
```
修改為`ES6`時，則需調整寫法如下：
```
let name = 'Pitt'
console.log(`My name is ${name}`)

// 同樣印出 My name is Pitt
```
最外圍使用倒鉤符號包裹，先用$符號聲明，再用{}包覆變數。
### 多行字串
`ES5`寫法中，多行字串使用`\n`，但在`ES6`中，若使用倒鉤符號包裹後，自動換行即可，寫法如下：
```
let msg =
`<span>
  Hello World!
</span>`
```
## 展開與其餘運算符
用法為`...`，常用於陣列合併或是複製陣列。
合併用法如下：
```
let a = [1, 2, 3]
let b = [4, 5, 6]

let c = [...a, ...b]
console.log(c)
// 印出 [1, 2, 3, 4, 5, 6]
```
複製陣列後修改其中的值：
```
let d = [...c]
d[2] = 7
console.log(d)

// 印出 [1, 2, 7, 4, 5, 6]，而此時的c陣列是不受影響的
```
## import module
在`ES6`語法中，JS開始原生支援模組系統，用法如下：
先建立一個引用的`HTML`檔案，同時引用js檔案時，需注意`type = module`，寫法如下：
```
<script src="./index.js" type="module"></script>
```
準備一個`index.js`檔案來載入導出的資料，再準備一個`module.js`來導出資料
```
// index.js

import { data, userData } from './module.js';

console.log(data)
console.log(userData)
```
```
// module.js

export const data = [1, 2, 3]

export const userData = [{
  'id': 'first',
  'name': 'Pitt'
}]
```
如此便完成最基礎的`import`和`export`。