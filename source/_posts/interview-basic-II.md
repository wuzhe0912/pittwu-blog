---
title: 常見面試問題 II
date: 2020-01-05 17:36:59
tags:
  - Interview
---
面試問題彙整 II。
<!--more-->
## ES6
- 定義 var、let、const 
在 ES6 中，新增了兩種變數宣告方式 let、const，傳統的 var 容易污染到全域變數，因此在 ES6 調整了變數應用在作用域的範圍，通常是以 function 為分界，使用 let 在 function 內宣告變數後，可以避免離開 function 仍被污染，另一方面也可以再次賦值。const 定義為常數，不能重複賦值，而且也要求一定要賦值。

- 請簡單說明 arrow function？
寫法最典型的狀況是省略了 function 改用 => 取代，但除了這點外，若參數僅有一個的情況下可以省略()，另外若 return 只有一行的狀況下可以省略 {}，但這邊是看團隊的 coding style，我個人不習慣太省略的寫法。

- 請問什麼是 this？
this 就是 function context，在 ES5 的時候，是根據 function 本身的情境來決定指向的對象，如果 function 沒有做其他處理，那就會自動往外尋找全域變數內，有沒有被宣告變數。但如果 function 被賦值給物件，那就會向物件內找，又或者執行事件，那就會向綁定的Dom元素找。但在 ES6 的 arrow function 下，任何情境都會指向全域變數。
  - 指向全域
  ```
  var name = 'Pitt'

  var myName = function () {
    console.log(this.name)
  }

  myName()
  ```
  - 指向物件
  ```
  var name = 'Pitt'

  var myName = function () {
    console.log(this.name)
  }

  var newName = {
    name: 'Nini'
  }
  newName.myName = myName
  newName.myName()
  ```

## JavaScript
- 請說明 Hoisting？
一般理解中，JS 在執行時應該是按順序執行，但因為 JS 本身有宣告提升的特性，因此如果沒有宣告變數，但先進行賦值，被挪到後方的變數會被提升到前方進行賦值。
```
item = 10
console.log(item)  // 印出10
var item
```
但宣告變數這個動作是沒有提升的效力的，所以如果宣告變數和賦值都挪到後方，則會失效。
```
console.log(item) // 印出 undefined
var item = 10
```
同理，function 中也有相同的特性
```
test()
function test () {
  console.log('Hello!') // 印出 Hello
}
```
賦值後失效，因為僅只有變數被提升
```
test()
var a = function test () {
  console.log('Hello!') // undefined
}
```

## Vue.js
- 如何理解 MVVM？
操作 DOM 這件事交給框架處理，我們負責改變框架中的資料，驅動資料去改變頁面重新渲染。而對前端來說，處理的是view層和 viewModel，而 model 層則是後端在處理。舉例來說，view 層就是房子的基礎結構，由 HTML 和 CSS 組成，但是我們居住的房屋不太可能是一個毛胚屋，所以更細節的裝修與動線規劃就必須仰賴設計，也就是 viewModel 層。當從 A房間 => B房間，兩間房間有什麼差異？這些東西，在過去的寫法是相對麻煩的，需要先抓到每個節點，再根據那個節點加入執行動作。但在目前的框架幫助下，我們透過資料綁定，只要改變資料，頁面也就會隨之改變，可以只單向綁定，改變資料就調整 class，或是雙向綁定(v-model)，我們 input 輸入什麼內容，而頁面就顯示相對應內容。

- Vue 的生命週期
  - created 和 mounted 的差異？
  created 在頁面開始創建前，它會先進行 init data，但這個時候頁面結構是還沒開始渲染的，所以如果是呼叫 api 的資料，多數要等到 mounted 階段，因為這個時候 $el 已經被掛載上去了，頁面也已經渲染完成。
  - updated
  簡單說就是更新 data，常見的案例是，我們透過 @click 事件執行 methods 內的函數，在函數中會去改變 data 內的某個資料來達成頁面更新。
  - destroyed
  我的理解是銷毀節點，常用在 v-if，當狀態改變時，data 中的資料可能由 false => true 或是相反，這個時候頁面的綁定對象也就要隨之改變。譬如說，我在頁面 init 時 data 的 loading 掛為 false，但我在 created 中先執行一個呼叫 api 的函數，同時在函數中先將 loading 轉為 true，一但資料拿到後，再轉回 false，也就是我在資料取得前，構建了 loading 的節點，等到取得資料後在銷毀它。

- Vuex 的原理
在 Vuex 當中，state 會存放初始化的資料，當 component 呼叫 Vuex 中寫好的函數，首先會到 actions 找對應的函數，這時會去呼叫 api 的資料，當然如果 component 呼叫時有傳入參數，那這個參數就透過 payload 傳入函數。當 api 的資料取得後，除了回傳給 component，也有可能透過 commit 的方式來改變 mutations 內的函數，而當 mutations 這時就會將傳來的資料賦值給 state，進而改變狀態。

- 什麼情境下，需使用 Vuex？
Vue 的專案中，通常會有多個 component 組成，尤其專案越大組成結構就越複雜，有些資料會是多個頁面需要共用，這個時候不可能仰賴父子組件傳值，萬一組件和組件之間隔了好幾層，那效益就太差了，這時通常就會抽離到 Vuex 進行狀態管理。舉例來說，常見的使用者帳號狀態，因為涉及到登入登出的情境，login 和 logout，頁面呈現狀態會有所差異，不可能在每一個需要使用 userInfo 的 component 都寫一遍呼叫 api 的函數，所以無論是 init 的資料或是呼叫 api 的函數，這時候就會統一寫在 Vuex，方便要使用的人直接調用。

## HTML
- HTML5 特色
廣義而言，新增了相當多不同領域功能的 tag，語意化，諸如 header、main、section 之類的語易化標籤，善用這些標籤能對網站 SEO 能更加友善。多媒體，隨著 HTML5 開始支援播放影片音樂，也成了壓垮 flash 的最後一根稻草。Canvas 除了讓網頁直接繪出圖形，也讓許多遊戲產業獲得更好的發展。其他則還有支援移動裝置的 touch 事件、地理資訊紀錄以及雙向溝通的 websocket(但存有安全性疑慮)。

## CSS
- 請解釋 * { box-sizing: border-box; }？並且說明使用它的好處？
`box-sizing`：意指，當元素計算寬度和高度時，`border` & `padding` 會內含還是外加。

`content-box`：預設的屬性，會使 `border` & `padding` 添加額外的寬高(外加)。
`border-box`：為了改善前面這種不友好的設計，將 `border` & `padding` 塞進元素本身(內含)。

因此我們通常建議在初始化樣式時，加入到 `*`。
```
* {
  box-sizing: border-box;
}
```

## Web 相關
- 什麼是跨域問題，如何解決？
當 ajax 發出請求時，瀏覽器會要求當前網頁與 server，基於安全性必須同源。也就是協議(http、https)、域名(abc.com)、端口(8080)，三者皆必須一致，只要任一者不同，就是不同源。當然，如果是 cdn 引入 js 的框架，或是 css 引入圖片網址，可以無視這個規則。任何跨域行為，都應該經過 server 端允許，若未經允許即可實現跨域，代表網站本身已存在重大漏洞。解決方案，主要有三種，JSONP 和 CORS 以及代理，JSONP 透過 script 導入目前比較少使用，CORS 則是由後端處理，前端直接使用，代理則需要特定設置(vue => vue.config.js)。

- CORS 是什麼，它解決了什麼問題？
前端的一種跨域方式，由 server 端設置(通常是由後端設置在 http header)，前端可以直接使用，也可以只允許特定網址使用。

- 常見的 http 狀態碼？
```
200 => success
400 => 前端送錯 request，後端無法處理
401 => 少 token
403 => 沒有權限
404 => 找不到資源
500 => server 端掛了
502 => server 端某個功能爆了
503 => 維修
504 => api 請求超時
```

- 描述 cookies/localStorage/sessionStorage 的差別？
cookie 早於後兩者，用於瀏覽器 <=> server 之間的溝通，比較早期的做法會被用來暫時借用本地儲存，但隨著 HTML5 的出現，本地儲存資料已改用後兩者。一般來說使用 localStorage 居多，畢竟使用者下次登入網頁時間未知，所以通常會選擇永久儲存，而 sessionStorage 則是關閉網頁即失效。

## 性能優化
- 前端效能優化可以怎麼做？
壓縮文件大小，除了 webpack 本身的打包壓縮，圖片可以透過 tinypng 來壓縮圖片大小，另外顏色豐富的圖片或較大的圖片，優先考慮 jpg。如果要針對性地去優化，可能要使用開發工具中的 performance 來檢查。

- 前端如何縮短呼叫 API 所花費的時間？

## 程式碼題目

## 框架比對
- Vue 和 React 的差異？