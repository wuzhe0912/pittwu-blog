---
title: 常見面試問題：程式碼相關
date: 2019-12-11 13:59:36
tags: Interview
---
程式碼相關面試問題彙整
<!--more-->
### foo 的值是什麼？
```
var foo = 10 + '20';
```
number + string => string 呈現，若兩者都是 number，則進行計算。
```
1020
```

### 實作符合下面的函式
```
plus(2, 5); // 7
add(2)(5); // 7
```
第一題是一般函數的計算
```
function plus (val, val2) {
  return val + val2
}

console.log(plus(2, 5))
```
第二題不是一般的寫法，解法需使用到閉包的部分
```
function add (val) {
  return function (subVal) {
    return val + subVal
  }
}

console.log(add(2)(5))
```

### 下面的 statement(陳述式) 會回傳什麼？
```
"Hello World!".split("").reverse().join("");
```
split() 會將 string 進行分割，若有給定參數，則會根據該參數進行拆分並重組成陣列，譬如空格或是逗號，但因為這邊沒有，所以會根據每個字元拆，得到拆完的結果後，透過 reverse() 將陣列中的資料進行反轉，最後再透過 join() 將陣列中所有元素加入一個 string。
```
"!dlroW olleH"
```

### window.foo 的值是什麼？
```
( window.foo || ( window.foo = "bar" ) );
```
window.foo 在邏輯運算中，僅有一側被賦值，自然使用 bar，除非兩側都賦值，|| 判斷才會使用第一組。
```
bar
```

### 下面 foo.length 的值是什麼？
```
var foo = [];
foo.push(1);
foo.push(2);
```
宣告空陣列，依序透過 push() 塞值，array 中會拿到兩個元素，長度為2
```
console.log(foo)        // [1, 2]
console.log(foo.length) // 2
```

### 下面這段會印出什麼？
```
console.log('one');

setTimeout(function() {
  console.log('two');
}, 0);

console.log('three');
```
JS 本身為單執行緒，遇到同步工作時會依序執行，但非同步工作則會先丟到 task queue(任務等待序列)中，直到瀏覽器所有工作執行完，才會回頭來檢視 task queue。
```
one
three
two
```

### 下面的兩個 alerts 的結果會是什麼？
```
var foo = "Hello";

(function() {
  var bar = " World";
  alert(foo + bar);
})();

alert(foo + bar);
```
第一次 alert 的結果正常，因為 foo 可以找到全域變數，而 bar 則使用函數作用域內的變數。
```
Hello World
```
第二次 alert 時，foo 全域變數仍然存在，但是 bar 在函數內的變數已被銷毀，所以會變成 bar is not defined
```
bar is not defined
```