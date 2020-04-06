---
title: 常見面試問題：ES6
date: 2019-12-25 19:34:10
tags:
  - Interview
  - ES6
---
ES6 面試問題彙整。
<!--more-->
### 定義 var、let、const 
在 ES6 中，新增了兩種變數宣告方式 let、const，傳統的 var 容易污染到全域變數，因此在 ES6 調整了變數應用在作用域的範圍，通常是以 function 為分界，使用 let 在 function 內宣告變數後，可以避免離開 function 仍被污染，另一方面也可以再次賦值。const 定義為常數，不能重複賦值，而且也要求一定要賦值。

### 請簡單說明 arrow function？
寫法最典型的狀況是省略了 function 改用 => 取代，但除了這點外，若參數僅有一個的情況下可以省略()，另外若 return 只有一行的狀況下可以省略 {}，但這邊是看團隊的 coding style，我個人不習慣太省略的寫法。且 this 在 arrow function 下會全部被指向全域變數，而不需要考慮所處的狀況。