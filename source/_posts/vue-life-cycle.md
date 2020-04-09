---
title: Vue.js 生命週期簡單理解
date: 2020-02-13 00:16:03
tags: Vue.js
---
整理 Vue 的生命週期和用法理解。
<!--more-->
## 數據變化與更新
### beforeUpdate & updated
透過 @click 事件來達到數據更新的功能。
一開始頁面渲染 data 的初始資料，當點擊時觸發函式執行資料更新。
```
// template
p {{ msg }}
button(@click="update()")

// data 存放資料的初始值
data () {
  return {
    msg: 'Hello Vue!',
  }
},

// methods 存放執行的函式，透過點擊執行函式來更新資料
methods: {
  update () {
    this.msg = 'Hello Pitt!'
  }
}
```
## 銷毀
### beforeDestroy & destroyed
理解銷毀的概念，可以透過 v-if 來理解。當判斷為 false 時，本質上就等於銷毀該 Dom 節點。若預設 data 為 true，一開始頁面渲染時會出現，此時不會觸發 beforeDestroy & destroyed。但當點擊動作觸發轉為 false 時，則會觸發 beforeDestroy & destroyed，這時頁面上被綁定的對象也會消失，即可是為銷毀。
## 掛載順序
在 Vue.js 的生命週期中，當頁面渲染時，透過 console.log，可以看到依序為：
```
1. beforeCreate // 在這個階段 $el 和 $data 都尚未掛載 => undefined
2. created // $data 初始資料已載入，$el 尚未 => undefined
3. beforeMount // $el 依然為空 => undefined
4. mounted // $el 完成渲染出現資料
```