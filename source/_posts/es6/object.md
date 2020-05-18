---
title: ES6：類別與物件的基本觀念
date: 2019-12-02 23:25:48
tags: ES6
---
關於類別與物件的基本觀念。
<!--more-->
## 什麼是類別與物件
### 舉例來說
```
類別，可以理解為設計圖

物件，則是根據設計圖製造出來的實體
```
#### 馬克杯
```
我們可以請設計師，繪出一個馬克杯的設計稿

接著，工廠可以根據設計圖，製造出無數的馬克杯
```
#### 回到程式語言
```
我們可以使用一個類別設計，產生無數個物件實體
```
#### 關鍵字
```
// 在類別中，我們會使用到一些關鍵字
class、constructor

// 在物件中，則會使用到
new
```
## 定義類別並產生物件
### 寫法
```
// 定義一個類別 Car
class Car {}

// 利用已經定義好的類別，產生新的物件
// 再將 new Car() 產生的新物件，放入變數中
let car1 = new Car();
let car2 = new Car();
```
### 定義建構式 (constructor)
- 建構式：建立新物件時被呼叫的函式
#### 寫法
```
// 定義一個類別 Car
class Car {
    // 在類別中，定義建構式
    constructor() {
        console.log("呼叫建構式");
    }
}

// 利用已經定義好的類別，產生新的物件
let car1 = new Car();
let car2 = new Car();

// 上述流程
定義類別 -> 先呼叫建構式 -> 執行完建構式內容 -> 產生新物件 -> 放入變數
```
### 定義與存取屬性 (Attribute)
#### 在建構式中建立屬性
```
constructor (參數) {
    this.屬性名稱 = 初始資料;
}
```
#### 寫法
```
// 定義一個類別 Car
class Car {
    // 在類別中，定義建構式
    constructor() {
        this.color = "blue"; // 建立新屬性 color，指定資料為"blue"
    }
}

// 利用已經定義好的類別，產生新的物件
let car1 = new Car(); // 新物件擁有 color 屬性，資料為"blue"
let car2 = new Car(); // 新物件擁有 color 屬性，資料為"blue"
```
#### 透過參數，彈性建立新屬性，達到多個物件的差異
```
// 定義一個類別 Car
class Car {
    // 在類別中，定義建構式
    constructor(color) {
        // 透過新物件的參數，使屬性在賦值同時產生變化
        this.color = color;
    }
}

// 利用已經定義好的類別，產生新的物件
let car1 = new Car("blue"); // 新物件擁有 color 屬性，資料為"blue"
let car2 = new Car("red"); // 新物件擁有 color 屬性，資料為"red"
```
#### 同一物件，更新屬性
```
// 定義一個類別 Car
class Car {
    // 在類別中，定義建構式
    constructor(color) {
        // 透過新物件的參數，使屬性在賦值同時產生變化
        this.color = color;
    }
}

// 利用已經定義好的類別，產生新的物件
let car1 = new Car("blue"); // 新物件擁有 color 屬性，資料為"blue"
console.log(car1.color); // 取得屬性的資料，印出 blue

car1.color = "red" // 更新屬性資料
console.log(car1.color) // 取得新的屬性資料，印出 red
```
### 定義、呼叫方法 (Method)
#### 寫法
```
// 定義一個類別 Car
class Car {
    // 在類別中，定義建構式
    constructor(color) {
        // 透過新物件的參數，使屬性在賦值同時產生變化
        this.color = color;
    }
    // 定義一個 Run 方法，透過物件呼叫，並執行內部程式碼
    run() {
        console.log("Running");
    }
}
// 產生新物件，並且物件同時擁有 color 屬性和 run 方法
let car1 = new Car("blue");
```
#### call method
```
// 定義一個類別 Car
class Car {
    // 在類別中，定義建構式
    constructor(color) {
        // 透過新物件的參數，使屬性在賦值同時產生變化
        this.color = color;
    }
    // 定義一個 Run 方法，透過物件呼叫，並執行內部程式碼
    run() {
        console.log("Running");
    }
}
let car1 = new Car("blue"); // 產生新物件，並且物件同時擁有 color 屬性和 run 方法
car1.run(); // call run method，並執行 run 內部的 code，印出 "Running"
```
#### 綜合應用
```
class Car { // 定義一個類別 Car
    constructor(color) {
        this.color = color;
        this.speed = 0; // 初始化車子速度為 0
    }
    run(val) {
        this.speed = val;
        console.log(this.color + "Car Running at" + this.speed + "km/hr" );
    }
    stop() {
        this.speed = 0;
        console.log(this.color + "Car is stopped");
    }
}

// 產生一個新物件，擁有 color、speed 屬性和 run、stop 方法
let car1 = new Car("red");
car1.run(80);
car1.stop();
```