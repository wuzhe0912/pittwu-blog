---
title: ES6：解構賦值
date: 2020-02-09 23:22:15
tags: ES6
---
解構賦值簡單來說，就是輕便且快速地取出元素。
<!--more-->
## 陣列解構
若有一個陣列如下
```
let numArray = [2, 4, 6]
```
可以透過下面兩種方法進行解構
```
// 方法一
let [subNode, subItem] = numArray

// 方法二
let subNode
let subItem
[subNode, subItem] = numArray
```
這兩種方法，都會拿到相同結果。當陣列解構後，就可以進行一般運算或處理。
```
let res = subNode * subItem
console.log(res)  // 印出8
```
## 預設值
如果無法預期拿到陣列內容為何？透過預設值，可以避免缺少值的問題。
例如：
```
let numArray = [2, 4]

let [first, second, third] = numArray

first + second + third  // 這邊做計算的話，會產生 NaN
```
但如果透過預設值為0，則能避免上面的問題
```
let [first, second, third = 0] = numArray
```
## 忽略元素
意指，僅調用陣列中某一元素。可以透過前面的值為空，但逗號保留的方式來進行忽略元素。
```
let numArray = [2, 4, 8, 10]

let [, , , item] = numArray

console.log(item)
```
## 變數交換
透過陣列的形式，進行陣列解構的變數交換
```
let a = 2;
let b = 4;

[a, b] = [b, a]
console.log(a) // 印出 4
console.log(b) // 印出 2
```
## 剩餘部分重組
去除原始陣列中部分元素，透過 `...` 的方式，將剩餘的元素重組成新陣列。
```
let numArray = [1, 3, 5, 7, 9]

let [first, ...node] = numArray

console.log(node) // 印出 [3, 5, 7, 9]
```
## 物件解構
類似陣列解構，將變數快速賦值。
```
let itemObj = {
  a: 10,
  b: 20
}

let {a, b} = itemObj
console.log(a) // 印出 10
console.log(b) // 印出 20
```
物件解構，同樣可以給予預設值。
```
let {a, b, c = 0} = itemObj
```
改變變數名稱，解構時，冒號左側的 `key` 值必須維持和物件中相同，但右側可以命名新的變數。
```
let itemObj = {
  a: 10,
  b: 35
}

let {a:x, b:y} = itemObj
console.log(y) // 印出 35
```
## 解構函式
透過解構傳進來的參數，精簡程式碼
```
function distance(point) {
  let {x, y} = point
  return Math.sqrt(x*x + y*y)
}
```
但是解構函式，更進階的用法是，在參數內直接解構
```
function distance({x, y}) {
  return Math.sqrt(x*x + y*y)
}
```
因此既能給予預設值
```
function distance({x = 0, y}) {
  return Math.sqrt(x*x + y*y)
}
```
也能重新命名
```
function distance({x:a, y}) {
  return Math.sqrt(a*a + y*y)
}
```