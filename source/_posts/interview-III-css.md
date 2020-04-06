---
title: 常見面試問題：CSS
date: 2019-12-06 20:44:59
tags:
  - Interview
  - CSS
---
CSS 面試問題彙整。
<!--more-->
### 請解釋 * { box-sizing: border-box; }，並且說明使用它的好處？
`box-sizing`：意指，當元素計算寬度和高度時，`border` & `padding` 會內含還是外加。
`content-box`：預設的屬性，會使 `border` & `padding` 添加額外的寬高(外加)。
`border-box`：為了改善前面這種不友好的設計，將 `border` & `padding` 塞進元素本身(內含)。
因此我們通常建議在初始化樣式時，加入到 `*`。
```
* {
  box-sizing: border-box;
}
```

### 倘若有不同的樣式表 (stylesheets)，該如何整併到網站？ 
以 Vue 專案結構為例：
```
- assets
  - scss
    - color.scss
    - mixin.scss
    - share.scss
    - style.scss
```
將共用顏色和共用函數(breakpoint、position)進行拆分，兩者都 @import 到 share.scss，各 component 的 style.scss 直接 @import share.scss。同時再將 share.scss @import 到 style.scss，在 assets/scss/style.scss，被視為權重最高的樣式表，除了進行樣式初始化，也是用來處理特定需求(譬如覆蓋套件的樣式)，並將其註冊到 main.js。