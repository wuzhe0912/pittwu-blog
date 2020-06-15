---
title: 實作 React 購物車
date: 2020-05-11 23:35:15
tags:
  - React
---
客觀來說，沒有特別偏好使用`React`，但因為 Vue 3.0 的走向會朝向函數式編程的趨勢，這點似乎會和 React 目前的境況不謀而合，因此算是透過學習`React`來提前為 Vue 3.0 預作準備。再者，和好友聊天之餘，對方也希望我能偶爾摸摸`React`，盛情難卻之下，也算是學習契機。另外入門`React`的基礎知識後，開始嘗試實作一些功能，方便進一步理解`React`的應用。
<!--more-->
## 環境搭建
```
npx create-react-app react-store

cd project

yarn start
```

## React 元素
### 屬性名稱
一些 HTML 標籤的屬性名稱，原先是全小寫，但在 React 的環境下，若屬性為兩個單字，統一需要改為駝峰式名稱才能正常運作。譬如 `input` 標籤中 `readonly` => `readOnly`。

### 保留字
部分 HTML 標籤使用的名稱會和 React 產生的元素出現衝突，最常見的就是 `class`、`for`，所以需要使用 React 規範的名稱，`class` => `className`、`for` => `htmlFor`。

### 在 JS 中撰寫 CSS
這邊還真的寫起來蠻卡的，和過往撰寫 css 的做法有一定差異，使用 style 物件包裹的方式來添加屬性的樣式，結尾自然也就改為 `,` 而非 `;`。css 的屬性名稱，如果原先用 `-` 隔開者，則改為駝峰式名稱。如下：
```
const textArea = React.createElement("input", {
  style: {
    color: 'blue',
    border: '1px solid green',
    borderRadius: '6px'
  }
});
```

### 設定事件
- 寫法1
```
const clickEvent = () => {
  console.log("arrow");
};

const textArea = React.createElement("input", {
  onInput: clickEvent
});
```
- 寫法2
```
const textArea = React.createElement("input", {
  onInput: () => {
    console.log("arrow");
  }
});
```

### 傳入多個元素
在 createElement 中第三個參數可以使用陣列形式呈現，這樣就能在畫面上渲染多個元素：
```
const item = React.createElement('li', {}, '第一項')
const subItem = React.createElement('li', {}, '第二項')

const target = React.createElement('ul', {}, [
  item,
  subItem
])
```
當元素過多時，這種 React.createElement 其實並不適合維護與開發，因此才會導入 JSX 語法來進行撰寫。

## JSX 語法