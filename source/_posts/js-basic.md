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
## JS 作用域
JS 屬於靜態作用域，變數在語法解析時，就已經確定作用域，且不再改變。
```
let val = 1
function a() {
  console.log(val)
}
function b() {
  let val = 2
  a()
}
b()
```
雖然在 b() 當中重新對 val 進行賦值，但因為前述靜態作用域的特性，不會觸發這個改變後的賦值，而會選擇維持調用全域的變數。但如果今天這個是動態作用域，則結果相反，會等到函數執行後，取最新的變數。

## 範圍鍊
承前述，在函數內如果沒有變數，那倘若要印出變數，這個時候會向外尋找，先找到父層的函數，再沒有則往全域尋找，這樣的過程可以理解為一個範圍鍊。
```
var person = 'Pitt'

function first () {
  console.log(1, person) // Pitt
}

function second () {
  var person = 'Nini'
  console.log(2, person) // Nini
  function third () {
    var person = 'Yumi'
    console.log(3, person) // Yumi
  }
  first()
  third()
}

first()
second()
```
第一次執行 first() 時，因為這個函數內沒有變數 person，所以會向全域尋找，賦值為 Pitt，接著執行 second()，這時函數內 person 已經有值為 Nini 了，所以會印出 Nini，但這時再次執行 first()，因為靜態作用域的特性，所以 second() 的變數不會影響到 first()，因此仍會向全域尋找印出 Pitt。最後則是執行 third()，因為 third() 內同樣有變數，所以會印出 Yumi。

倘若我將 third() 中的變數註釋掉，那 third() 會先向父層 second() 尋找印出 Nini，如果 second() 也被註釋掉則會尋找全域變數印出 Pitt。

## 提升
JS 的運行可以拆解為兩階段，分別是創造與執行：
```
var name = 'Pitt'
console.log(name) // 印出 Pitt
```
在變數中，上面這段 code 在實際運作上，需要理解為
```
// 創造
var name

// 執行
name = 'Pitt'
console.log(name)
```
而函數又和變數不同，在創造階段就會指給記憶體，陳述式如下：
```
getName()
function getName () {
  console.log('string')
}
```
上面這段之所以能正常運行印出 console.log，而不會報錯，在於以下邏輯
```
// 創造
function getName () {
  console.log('string')
}

// 執行
getName()
```
但需要注意的是，這種 JS 提升的性質，在表達式時需要注意撰寫順序問題
創造階段 => 函數優先，其次才是變數
- success
```
name = 'Pitt'
console.log(name) // 印出 Pitt
var name

相等於

// 創造
var name

// 執行
name = 'Pitt'
console.log(name) // 印出 Pitt
```
- error
```
console.log(name) // 印出 undefined
var name = 'Pitt'

相等於

// 創造
var name

// 執行
console.log(name) // 印出 undefined，還未拿到賦值，只拿到預設的 undefined
name = 'Pitt'
```
- e.g.
```
whoseName()
function whoseName () {
  if (name) {
    name = 'Nini'
  }
}
var name = 'Pitt'
console.log(name)

相等於

// 創造
function whoseName () {
  console.log(1, name) // => 拿到預設的 undefined，因此不往下走判斷
  if (name) {
    name = 'Nini'
  }
}
var name

// 執行
whoseName()
name = 'Pitt'
console.log(name) // 印出 Pitt
```

## Event queue
JS 本身是單執行緒，因此非同步的事件在 JS 的執行過程中，會被放置到事件等待序列中，直到其他同步事件先被執行完成，才會被執行。
```
function a () {
  console.log(1)
}

function b () {
  console.log(2)
}

function c () {
  console.log(3)
  setTimeout(() => {
    console.log(4)
  }, 0)
}

function Doing () {
  a()
  c()
  b()
}

Doing() // => 依序印出 1 3 2 4
```

## 物件取值
除了過往用使用 . 來連結取值外，object也可以透過 [] 來進一步取值：
```
var obj = {
  name: 'Pitt',
  2: 2
}
console.log(obj.name)
console.log(obj[2])
```
如上，這種 index 特殊的狀況下，用 . 是無法取值，必須仰賴 []。此外，還有一種寫法如下：
```
var obj = {
  name: 'Pitt'
}
var newName = 'name'
console.log(obj[newName])
```
宣告變數，並將它的賦值和對應的物件屬性相同，就可以透過 [] 進行取值。物件新增同理，使用 . 或是 [] 都可以增加物件中尚未存在的欄位。前面加入 delete 則是刪除對應的欄位。
```
var obj = {
  name: 'Pitt'
}
obj.weapon = 'arrow'
obj['archer'] = 'Nancy'
console.log(obj)
//
obj = {
  name: 'Pitt',
  weapon: 'arrow',
  archer: 'Nancy'
}
```
```
delete obj.weapon
delete obj['archer']
console.log(obj)
//
obj = {
  name: 'Pitt'
}
```
*tips：變數無法被刪除，但屬性可以
```
var a = 10 // 宣告後才是變數
b = 20 // 賦值只能算是屬性

delete a
delete b

console.log(window.a) // 10
console.log(window.b) // undefined
```

## 閉包
什麼是閉包？簡單說，就是當 JS 的 function 因為GC(垃圾回收)機制觸發，導致函數執行完後，內部作用域內容會被銷毀。這時可以透過子函數來保存我們需要的變數或內容。
```
function deposit () {
  var cash = 100
  return function (coin) {
    cash = cash + coin
    return cash
  }
}
```
因為子函數沒有被銷毀，加上範圍鍊的原則，子函數調用外面父函數的變數 cash，因此父函數作用域在閉包的狀況下被保存下來，同時後續宣告不同變數時，也可以獨立調用這一組函數而不用擔心互相污染。
```
function deposit () {
  var cash = 100
  return function (coin) {
    cash = cash + coin
    return cash
  }
}

var PittPay = deposit()
console.log(PittPay(100)) // 200
console.log(PittPay(100)) // 300
console.log(PittPay(100)) // 400

var KimiPay = deposit()
console.log(KimiPay(200)) // 300
console.log(KimiPay(200)) // 500
console.log(KimiPay(200)) // 700
```

## Promise
ES6 的新方法，本質上是一個物件，用來處理非同步的方法。