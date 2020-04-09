---
title: 常見面試問題：CSS
date: 2019-12-06 20:44:59
tags:
  - Interview
  - CSS
---
CSS 面試問題彙整。
<!--more-->
### 基礎語法
#### Q：簡單聊聊 box-model
box-model 可以理解為前端工程師製作網頁的一種工具，透過它可以定義網頁上元素的位置，進而達到排版的目的。從內到外，環繞它身邊有三個語法，padding、border、margin。其中前兩者預設會影響到元素，因此在 box-sizing 會採用另一個模式 border-box，來避免不必要的計算。

#### Q：請解釋 * { box-sizing: border-box; }，並且說明使用它的好處？
![](/images/box-sizing.png)
box-sizing => 意指，當元素計算寬度和高度時，border & padding 會內含還是外加。
content-box => 預設的屬性，會使 border & padding 添加額外的寬高(外加)。

但前述這個設計，會使前端開發時被迫需要一直計算寬高，為了改善這個開發體驗，所以改用以下模式：
border-box => 將 border & padding 塞進元素本身(內含)。
因此我們通常建議在初始化樣式時，加入到 `*`。
```
* {
  box-sizing: border-box;
}
```
- [圖片詳解](https://zh-tw.learnlayout.com/box-sizing.html)

### 情境處理
#### Q：如何處理水平垂直置中？
在 flexbox 語法中，處理這個需求並不困難：
```
div {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

但如果不使用 flexbox 就會比較麻煩，首先水平置中，常用兩種方法：
```
div {
  margin: 0 auto;
}

div {
  text-align: center;
}
```

至於垂直置中的方法有數種，但我這邊只記錄兩種：
1. line-height
2. vertical-align: middle;

但基本我個人現在只用 flexbox 語法來處理，未來會再加上 grid。

### 專案結構
#### Q：倘若有不同的樣式表 (stylesheets)，該如何整併到網站？ 
以 Vue 專案結構為例：
```
- assets
  - scss
    - color.scss
    - mixin.scss
    - share.scss
    - style.scss
```
將共用顏色和共用函式(breakpoint、position)進行拆分，兩者都 @import 到 share.scss，各 component 的 style.scss 直接 @import share.scss。同時再將 share.scss @import 到 style.scss，在 assets/scss/style.scss，被視為權重最高的樣式表，除了進行樣式初始化，也是用來處理特定需求(譬如覆蓋套件的樣式)，並將其註冊到 main.js。

#### Q：如何處理專案 RWD
- 共用參數
假設 UI/UX 為首次合作，因此會先討論網站的色系與主要的共用間距(當然如果是經驗豐富的 UI/UX，這一步可以忽略)，因為斷點、三輔一主的色系、常用間距，需先準備在 scss 作為共用參數。至於斷點，現在市面上，320px 尺寸的手機(iPhone5)，雖然市佔率已經相當低，但目前開發上仍會考慮保留，因此預期我的斷點會設計：
```
360 => 兼容小尺寸 android & iPhone5
480 => 主流手機
768 => pad
pc => 這個沒有固定，視討論而定，通常1024(但其實也可以用 padPro來命名)
```

- UI Framework
主要分為兩種狀況，手刻或者用框架：
  - 手刻：目前主流是用 flexbox(目前兼容性已經極高了，哪怕是大陸冷門瀏覽器 QQ、UC)，至於 grid 目前來說，對移動裝置的支援還有疑慮，稍微舊一點的版本或是冷門瀏覽器都是不支援居多，還需要一點時間讓瀏覽器廠商跑步前進。
    - [瀏覽器市佔率](https://hackmd.io/tlJAS8MqQ0qqJ0wh3sggsw)
  - 框架：這就看團隊的技術選型，根據團隊採用不同的前端框架，相應的框架也會有差異，以 Vue 來說，目前海外社群投票最大宗是 Vuetify，其次是 element-ui(但不支援 RWD，主要用於 CMS)，Tailwind CSS 正在追趕，Bootstrap 這我不太確定，需等 v5 之後的版本觀察。
    - [Tailwind CSS](https://hackmd.io/mmVT15NkT8KZJTR5octHlA)

  至於更細節的開發，就視實際的設計稿而定，這邊不再贅述了。