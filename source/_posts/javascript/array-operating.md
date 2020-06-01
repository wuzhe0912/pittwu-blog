---
title: JS 陣列&物件 操作方法
date: 2020-01-13 17:38:48
tags:
  - JavaScript
---
處理資料是開發 JS 過程中，常見的場景，如果是常用的資料格式，固然處理速度很快，解決方法也不難，但現實中，常會有一些特殊的情境。因此記錄各種操作方式，除了加深個人 JS 知識，未來若有類似場景，也能加速開發效率。
<!--more-->
## Array
資料：
```
const playerData = [{
    player: 'Pitt',
    job: 'witcher',
    hp: 500
  },
  {
    player: 'Mio',
    job: 'archer',
    hp: 200
  },
  {
    player: 'Ines',
    job: 'warrior',
    hp: 600
  },
  {
    player: 'Sue',
    job: 'archer',
    hp: 200
  },
  {
    player: 'Nicole',
    job: 'tank',
    hp: 800
  }, {
    player: 'Nancy',
    job: 'mage',
    hp: 200
  }, {
    player: 'Kuni',
    job: 'priest',
    hp: 350
  },
]
```

### forEach
將陣列中所有元素透過函式進行運算，相較於過往的`for`迴圈寫法，更為簡潔：
```
let totalHP = 0
playerData.forEach((node, index) => {
  totalHP = totalHP + node.hp
})
console.log(totalHP) // 2850
```
改變陣列中所有元素的值
```
playerData.forEach((node, index) => {
  node.hp = node.hp + 250
})
console.log(playerData)
```

### for(舊式用法，現多已不使用)

### filter
尋找陣列中所有符合函式條件的元素進行回傳
```
const filterArr = playerData.filter((node) => {
  return node.job === 'archer'
})
console.log(filterArr) // Mio、Sue
```
若沒有符合條件的元素，則回傳空陣列
```
const filterArr = playerData.filter((node) => {
  return node.hp < 50
})
console.log(filterArr) // []
```
判斷目錄餘數為1的條件：
```
const filterArr = playerData.filter((node, index) => {
  return index % 2 === 1
})
console.log(filterArr) // [Mio、Sue、Nancy]
```

### find
近似於`filter`，但不同的點在於當找到陣列中第一個符合的元素時，即進行回傳，若陣列中沒有符合的值，則回傳`undefined`。
```
const findArr = playerData.find((node) => {
  return node.hp > 1000
})
console.log(findArr) // undefined
```
找到陣列中第一個符合的值即回傳(適合用於進行`formatter`翻譯)：
```
const findArr = playerData.find((node) => {
  return node.job === 'archer'
})
console.log(findArr) // Mio
```

### map
`map()`方法透過函式，將原始陣列進行重組後，回傳一個新的陣列：
```
const mapArr1 = playerData.map((node) => {
  return node.hp + 100
})
console.log(mapArr1)
```
但這個方法本身有兩個條件，其一，一定會回傳值，若條件不符合則回傳`undefined`。其二，新的陣列長度等於原始陣列長度。
```
const mapArr2 = playerData.map((node) => {
})
console.log(mapArr2) // 新陣列中會拿到全是 undefined
```
```
const mapArr3 = playerData.map((node) => {
  if (node.hp > 300) {
    return `血量大於300的職業：${node.job}`
  } else return `脆皮職業：${node.job}`
})
console.log(mapArr3)
```

### every
將陣列中所有元素帶入函式，並根據函式的要求進行判斷，若有一個元素不符合要求，則回傳`false`，反之，則回傳`true`。
```
const everyArray = playerData.every((node) => {
  return node.hp > 400
})
console.log(everyArray) // false
```

### some
和`every()`非常相近，同樣都是回傳`true、false`，不過差別在於判斷條件，`some()`僅需陣列中任一元素符合條件，即回傳`true`。當然，若全部條件皆不符合，則回傳`false`。
```
const somArray = playerData.some((node) => {
  return node.hp > 500
})
console.log(somArray)
```

## Object

## Reference
- [JavaScript Array 陣列操作方法大全 ( 含 ES6 )](https://www.oxxostudio.tw/articles/201908/js-array.html#array_every)
- [JavaScript 陣列處理方法](https://wcc723.github.io/javascript/2017/06/29/es6-native-array/#Array-prototype-some)
