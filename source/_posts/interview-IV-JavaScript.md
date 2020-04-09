---
title: 常見面試問題：Vanilla JS
date: 2019-12-27 00:34:44
tags:
  - Interview
  - JavaScript
---
JavaScript 面試問題彙整。
<!--more-->
### 基礎原理
#### Q：什麼是 DOM ?
我們所撰寫的 HTML 文件，瀏覽器會解析成樹狀結構，父層包覆子層形成一個 document 文件，每一個元素都可以理解為物件，DOM 操作就是透過 JS 去操作這些物件去執行行為。同時為了避免操作過程中，污染相同屬性的元素，譬如兩個 p 標籤，所以我們會透過 id 或 class 來指定操作：
```
document.getElementById(id)
document.querySelector(.className)
```

#### Q：請問什麼是 this？
this 雖然簡單來說就是函式內的執行情境(function context)，但需要拆成 ES5 和 ES6 兩種狀況來解釋。 

在 ES5 的時候，是根據 function 內的動作來決定 this 指向的對象，如果 function 沒有做其他處理，那就會自動往外尋找全域變數。e.g.：
```
var name = 'Pitt'

var myName = function () {
  console.log(this.name) // 這邊的 this 向函式外尋找變數
}

myName() // 印出 Pitt
```
但如果 function 被賦值給物件，那就會向物件內尋找變數。e.g：
```
var name = 'Pitt'

var myName = function () {
  console.log(this.name)
}

var newName = {
  name: 'Nini'
}

newName.myName = myName
newName.myName() // 印出 Nini
```
但如果 function 的執行被綁在 DOM 元素上，譬如 click 事件，那麼 this 就會被指向 DOM 上面相同名稱的變數。

但到了 ES6 的環境下，this 的指向不再這麼複雜，透過 arrow function 的自動綁定的特性，this 一律指向全域變數(window)。

#### Q：簡單說明立即函式(IIFE)
寫法格式如下，用小括號包裹，並在最後額外加上一個 ()。
```
// 兩種寫法都可以
(function () {
  console.log(1)
}())

(function () {
  console.log(2)
})()
```
他有兩個特點，一是會被立即執行，二是不管對立即函式是否命名，它都無法在函式再次被執行。基於函式作用域會銷毀變數的特性，立即函式執行完畢後，也不會保留其作用域內的變數，因此常用於限制變數的作用域。立即函式可以透過結尾 () 傳參，再加上其也是表達式的特性，因此也可以透過變數，接受它 return 的結果。
```
var test = (function (val) {
  return val
})('params')

console.log(test) // 印出 params
```
另外，因為立即函式的特性，連續兩個立即函式，會造成 ASI(自動補全分號)的規則沒有運作。因此在連續兩組立即函式的狀況下，需要自己補上分號。立即函式在一般開發比較少見，以我個人開發經驗是完全沒用過，但在框架的底層程式碼中，立即函式是有被使用的，通常用於保證變數有被正常掛載到全域變數上。

#### Q：簡單聊一下 JS 的事件傳遞機制


### JS 基礎三座大山之一：原型與原型鍊
#### Q：請簡單說明 JS 的原型
我們使用原型的目的，在於建立一個樣板或者說藍圖，透過這個樣板，我們放入屬性或方法，這樣可以快速建立 instance(實體)，e.g.：
```
function Person (name, type) {
  this.name = name
  this.type = type
}

var boy = new Person('pitt', 'boy')
var girl = new Person('kumi', 'girl')
```
另外，prototype 也可以優化 function 佔用的記憶體空間，e.g.：
```
function Person (name, type) {
  this.name = name
  this.type = type

  this.say = function () {
    console.log(`${this.name} is a ${this.type} say Hi!`)
  }
}

var boy = new Person('pitt', 'boy')
var girl = new Person('kumi', 'girl')

console.log(boy.say === girl.say) // false
```
雖然他們呼叫同一個函式，但卻各自佔用一個空間，在專案小的時候，可能差異不大，但大型專案或多或少會影響效能，因此可以透過 function prototype 來優化
```
function Person (name, type) {
  this.name = name
  this.type = type
}

Person.prototype.say = function () {
  console.log(`${this.name} is a ${this.type} say Hi!`)
}

var boy = new Person('pitt', 'boy')
var girl = new Person('kumi', 'girl')

console.log(boy.say === girl.say) // true
```

#### Q：簡單聊一下繼承
- ES5 的寫法，使用 Object.create() e.g.：
```
function Person (name, type) {
  this.name = name
  this.type = type
}

var boy = new Person('pitt', 'boy')
var girl = new Person('kumi', 'girl')

var man = Object.create(boy)
console.log(man)
```
對 man 這個變數來說，它繼承到的原型 Person 只是一個空物件，但因為原型練的特性，他會向上找 `__proto__` 的內容，這時候就會找到屬性與方法，e.g.：
```
console.log(man.name) // pitt
console.log(man.type) // boy
```
- 在 ES6 中引入新的寫法 class，e.g.：
```
class Player {
  // 使用 function constructor (建構式)
  constructor (playerName, hp) {
    this.playerName = playerName
    this.hp = hp
  }

  playerStatus () {
    console.log(`玩家名稱：${this.playerName}，玩家血量：${this.hp}`)
  }
}

var Pitt = new Player('Pitt', 200)
Pitt.playerStatus()
```
- 子類別也可以透過 extends && super 去繼承父類別的方法與屬性，e.g.：
```
// father
class Player {
  // 使用 function constructor (建構式)
  constructor (playerName, hp) {
    this.playerName = playerName
    this.hp = hp
  }

  playerStatus () {
    console.log(`玩家名稱：${this.playerName}，玩家血量：${this.hp}`)
  }
}
var Pitt = new Player('Pitt', 200)
Pitt.playerStatus()

// son
class BeginnerPlayer extends Player {
  constructor (playerName, hp, job) {
    super(playerName, hp)
    this.job = job
  }

  playerJob () {
    console.log(`玩家：${this.playerName}，血量：${this.hp}，初始職業為${this.job}`)
  }
}

var Lisa = new BeginnerPlayer('Lisa', 300, '初心者')
Lisa.playerJob()
```
但我在前職中幾乎沒有使用過。不過在 React 這個寫法是極其普遍的：
```
class Board extends React.Components {
  constructor (props) {
    super(props)
  }
}
```

### JS 基礎三座大山之二：閉包
#### Q：閉包
什麼是閉包？簡單說，就是當 JS 的 function 因為GC(垃圾回收)機制觸發，導致函式執行完後，內部作用域內容會被銷毀。這時可以透過子函式來保存我們需要的變數或內容。
```
function deposit () {
  var cash = 100
  return function (coin) {
    cash = cash + coin
    return cash
  }
}
```
因為子函式沒有被銷毀，加上範圍鍊的原則，子函式調用外面父函式的變數 cash，因此父函式作用域在閉包的狀況下被保存下來，同時後續宣告不同變數時，也可以獨立調用這一組函式而不用擔心互相污染。
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

#### Q：簡單說明 Hoisting？
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
而函式又和變數不同，在創造階段就會指給記憶體，陳述式如下：
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
創造階段 => 函式優先，其次才是變數
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

### JS 基礎三座大山之三：同步與非同步
#### Q：簡單說明 JavaScript 為何需要非同步？簡單聊一下對 call stack 跟 event loop 的理解
JS 的本質是單線程的語言，因為它的工作之一就是修改瀏覽器的 DOM 結構，如果多線程但同時修改同一個節點，會讓整體情況變得相當複雜，所以為了避免複雜的狀況發生，因此才會採用單線程。而非同步就是因應單線程的背景，假設某個動作需要等待 2 秒，瀏覽器當然不可能等在原地 2 秒，因此會先執行所有同步性的工作，而所有非同步性的工作則先放到 task queue(任務等待序列)。瀏覽器先執行同步性工作的環境，可以理解為包在 call stack 區間，瀏覽器先依序把 call stack 內的工作執行完畢，當它檢查到 call stack 為空時，接著前往 task queue 中將等待序列的工作丟到 call stack 依序執行。
```
瀏覽器檢查 call stack 是否為空
=> 否 => 回到 call stack 繼續執行
=> 是 => 進入 task queue 開始將非同步工作丟到 call stack 執行
```
這樣不斷循環的過程就是 event loop。e.g.：
```
console.log(1);
setTimeout(function() { // => 這種非同步的函式就是 callback
  console.log(2);
}, 0);
console.log(3);

// 依序印出 1 3 2
```

### 原生 API
#### Q：如何用 JS 原生方法實作回上頁的功能？
可以使用下述這兩種方法：
```
1. window.history.back // 回到上一頁
2. window.history.go(-1) // 指定回到上一頁，若傳入參數為 -2，則指定回到上二頁
```