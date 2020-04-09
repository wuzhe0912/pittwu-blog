---
title: ES6：宣告變數與箭頭函式
date: 2019-12-02 11:17:16
tags: ES6
---
記錄自己學習 ES6 語法，對比 ES5 和 ES3 如何解決問題。
<!--more-->
## var、let、const
```
早期 javascript 只能用 var 宣告變數

在 ES6 版本後，添加 let、const 兩種方式
```
### let、const
#### let 用於宣告變數，可以選擇賦值或不賦值
```
// 賦值
let a = 1;

// 不賦值
let b;
```
#### const 用於宣告常數(不作變動)，一定要賦值
```
const c = 10;
```
### 理解與應用
#### var
傳統使用 var 宣告變數時，變數的 Scope(即影響範圍)，通常以 function 本身為分界
```
for (var i=0;i<3;i++) {
	console.log("i:"+i);
}
console.log("outside i:"+i);
```
#### let (使用更嚴格的 Scope)
```
for (let i=0;i<3;i++) {
	console.log("i:"+i);
}
console.log("outside i:"+i); // error，i is not defined
```
#### const
```
let a; // 宣告變數，可以暫時不給資料
a = 2; // 變數中的資料可以變動

const b; // error，常數宣告時，必須給定資料

const b = 10; // 正確，宣告常數同時給予資料
b = 20; // error，不能更動常數中的資料
```
### 比對 ES5 和 ES6 寫法
```
// ES5
Object.defineProperty(window, "a", {
    value: 1,
    writable: false,
})
console.log(window.a)
```
```
// ES6
const a = 1
console.log(a)
```
除了精簡程式碼，也讓寫法變得更輕鬆。
## Arrow Function (箭頭函式)
### 第一種：(參數列表) => (回傳值)
#### 傳統函式寫法
```
let add = function (n1, n2) {
    return n1+n2;
};
```
#### 箭頭函式寫法
```
let add = (n1, n2) => (n1+n2)
```
### 第二種：(參數列表) => {函式內部程式}
#### 傳統函式寫法
```
let add = function (n1, n2) {
    return n1+n2;
};
```
#### 箭頭函式寫法
```
let add = (n1, n2) => {
    return n1+n2
};
```
### 額外範例
```
let f = () => (5);
let result = f();

console.log(result);

// 印出 5
```
```
let f = (message) => {
    console.log(message);
};

f("Hello, Pitt");

// 印出 Hello, Pitt
```
### 匿名函式
#### 傳統寫法
```
setTimeout(function(){
    console.log("過了5秒");
}, 5000);
```
#### 箭頭函式寫法
```
setTimeout(() => {
    console.log("過了5秒");
}, 5000);
```
### 參數預設值
#### 傳統寫法
```
function show(message) {
    if (typeof message === "undefined") {
        message = "default";
    }
    alert(message);
}

show("Pitt"); // 顯示 Pitt
show(); // 顯示 default
```
#### 箭頭函式寫法
```
// 若未給定參數資料，則直接採用等號後的賦值

function show(message="default"){
    alert(message);
}
show("Hello"); // 顯示 Hello
show(); // 顯示 default
```
#### 額外範例
##### 範例一：傳統寫法
```
function multiply(n1, n2=2) {
    return n1 + n2;
}

multiply(2, 3); // 回傳 6
multiply(); // 回傳 4
```
##### 範例一：箭頭函式寫法
```
let multiply=(n1, n2=2) => (n1+n2);

multiply(2, 3); // 回傳 6
multiply(); // 回傳 4
```
##### 範例二
後方的參數可以使用前方的參數進行運算
```
function combine(first="Pitt", last="Wu", name=first+" "+last) {
    console.log(name);
}

combine("Nini", "Ya"); // 顯示 Nini Ya
combine("Nini"); // 顯示 Nini Wu
combine(); // 顯示 Pitt Wu
```