---
title: 傳值 or 傳址 & 淺拷貝 or 深拷貝
date: 2020-05-17 23:49:14
tags:
  - JavaScript
---
關於 JS 傳值的基礎知識，以及如何透過淺拷貝和深拷貝來改善潛在的問題。
<!--more-->
### Pass by value(傳值)
當變數的值為原始型別時，JS 的行為模式會採用 Pass by value
```
// 宣告 a 變數的賦值為 10，並且在記憶體中 copy 出 10，賦值給 b

var a = 10
var b = a
console.log(b) // 10
```

上述的傳值適用以下原始型別：
```
String
Number
Boolean
Undefined
Null
```

### Pass by reference(傳址)
而在物件型別中，則是另一番情景：
```
var object1 = { num: 5 }
var object2 = object1
console.log(object2) // { num: 5 }

object1.num = 8
console.log(object2) // { num: 8 }
```
在物件型別中，object1 和 object2 的賦值在記憶體中都會指向同一個位置，因此如果改變 object1，則連帶會影響到 object2，它並不會像原始型別一樣，在記憶體中進行 copy。

適用以下型別：
```
Object
Array
```

### 淺拷貝(shallow copy)
為了改善前述的 Pass by reference 產生的問題，具體延伸出以下方法：

#### 處理 Array
- slice() 方法
```
// slice 會從括號內的索引位置開始進行 copy

var array1 = [1, 2, 3]
var array2 = array1.slice(0)
console.log(array2) // [1, 2, 3]

array1[1] = 4
console.log(array1) // [1, 4, 3]
console.log(array2) // [1, 2, 3]
```

- 展開運算符
```
var array1 = [10, 20, 30]
var array3 = [...array1]
console.log(array3) // [10, 20, 30]

array1[2] = 50
console.log(array1) // [10, 20, 50]
console.log(array3) // [10, 20, 30]
```
上述這兩個方法都可以避免 Array 在記憶中被指向同一個位置。

#### 處理 Object
- Object.assign
操作 Object 的資料時，不建議直接操作原始資料，而是 copy 出來再進行處理
```
var object1 = { num: 10 }
var object2 = Object.assign({}, object1)
console.log(object2) // { num: 10 }

object1.num = 100
console.log(object1) // { num: 100 }
console.log(object2) // { num: 10 }
```

#### 淺拷貝存在的問題
淺拷貝只能處理物件型別中的第一層，如果資料中包含第二層甚至更多層時，淺拷貝無法處理。
```
var object1 = {
  name: 'Pitt',
  player: {
    name: 'Nancy'
  }
}

var object2 = {...object1}
console.log(object2)  // { name: 'Pitt' player: { name: 'Nancy' } }

object1.name = 'Mike'
object1.player.name = 'Alisa'
console.log(object2.name)   // 'Pitt'
console.log(object2.player.name) // 'Alisa'
```
從上述可以看到，淺拷貝有辦法處理淺層資料，但更深層的資料他就無能為力了。

### 深拷貝(deep copy)
- 透過 Lodash 提供的方法來處理深拷貝
```
var object1 = {
  name: 'Amy',
  player: {
    name: 'Betty'
  }
}
var object2 = _.cloneDeep(object1)

object1.player.name = 'Alisa'
console.log(object1.player.name) // 'Alisa'
console.log(object2.player.name) // 'Betty'
```
[Jsbin](https://jsbin.com/vixabijenu/edit?html,js,console)
Lodash 提供的方法，成功對深層資料進行複製，避免記憶體中被同時指向一個位置。

### Pass by sharing
凡事皆有例外，而 JavaScript 這門奇怪的語言，更是容易出現例外，它並不完全屬於傳值或傳址，而是同標題的 Pass by sharing。在函式的環境中，會出現特殊狀況：
```
var num = { value: 20 }

function changeNum (val) {
  val = { value: 40 }
}

change(num)
console.log(num) // { value: 20 }
```
按照傳址的邏輯，物件型別被改變時，記憶體中該位置的值也隨之被改變。但在函式的環境下，外部變數卻不受影響，但如果採用更新的方式，則會受到影響，如下：
```
var num2 = { value: 20 }

function changeNum (val) {
  num2.value = 40
}

changeNum(num2)
console.log(num2) // { value: 40 }
```