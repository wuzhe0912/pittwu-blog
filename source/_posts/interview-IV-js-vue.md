---
title: 常見面試問題：Vue.js
date: 2020-01-06 00:40:15
tags:
  - Interview
  - Vue.js
---
Vue.js 面試問題彙整。
<!--more-->
### 如何理解 MVC 和 MVVM 差異？
- MVC
```
view => 前端(切版、套入動畫效果)
controller => 後端(控制 router)
model => 後端(database)
```
MVC 開始流行的原因，是為了解決早期程式碼混在一起的困境(義大利麵code)，前後端的程式碼都混在一起。因此 MVC 開始進行職責分工，view 負責畫面顯示，控管 router 和網站行為則由 controller，查詢資料庫的資料，則是 model，在這個時代前端工程師這個職缺才開始浮現出來(負責 view)。也因為 router 交由後端控制，因此這時期的網頁仍是多頁面式的網頁，切換頁面時是真的在切換不同頁面，會有明顯的 loading 時間。

- MVVM
```
view => 依然是前端(控制使用者看到的渲染頁面)
model => 後端
viewModel => 前端(改由前端控制 router)
```
進入 MVVM 時代，將原生 JS 操作 DOM 這件事交給框架處理，我們負責改變框架中的資料，驅動資料去改變頁面重新渲染。而對前端來說，處理的是 view 和 viewModel，而 model 則是後端在處理。這個時候已經轉型為 SPA(單頁式應用)，網站頁面切換時，看似切換不同頁面，但本質只是切換同一頁的不同區塊，因此這時網頁的流暢性與使用性會更加流暢。


### 試說明 Vue 的生命週期
  - created 和 mounted 的差異？
  created 在頁面開始創建前，它會先進行 init data，但這個時候頁面結構是還沒開始渲染的，所以如果是呼叫 api 的資料，多數要等到 mounted 階段，因為這個時候 $el 已經被掛載上去了，頁面也已經渲染完成。
  - updated
  簡單說就是更新 data，常見的案例是，我們透過 @click 事件執行 methods 內的函數，在函數中會去改變 data 內的某個資料來達成頁面更新。
  - destroyed
  我的理解是銷毀節點，常用在 v-if，當狀態改變時，data 中的資料可能由 false => true 或是相反，這個時候頁面的綁定對象也就要隨之改變。譬如說，我在頁面 init 時 data 的 loading 掛為 false，但我在 created 中先執行一個呼叫 api 的函數，同時在函數中先將 loading 轉為 true，一但資料拿到後，再轉回 false，也就是我在資料取得前，構建了 loading 的節點，等到取得資料後在銷毀它。
   
### 簡單解釋一下 Vuex 的原理
在 Vuex 當中，state 會存放初始化的資料，當 component 呼叫 Vuex 中寫好的函數，首先會到 actions 找對應的函數，這時會去呼叫 api 的資料，當然如果 component 呼叫時有傳入參數，那這個參數就透過 payload 傳入函數。當 api 的資料取得後，除了回傳給 component，也透過 commit 的方式來改變 mutations 內的函數，而 mutations 這時就會將傳來的資料賦值給 state，進而改變狀態。

### 什麼情境下，需使用 Vuex？
Vue 的專案中，通常會有多個 component 組成，尤其專案越大組成結構就越複雜，有些資料會是多個頁面需要共用，這個時候不可能仰賴父子組件傳值，萬一組件和組件之間隔了好幾層，那效益就太差了，這時通常就會抽離到 Vuex 進行狀態管理。舉例來說，常見的使用者帳號狀態，因為涉及到登入登出的情境，login 和 logout，頁面呈現狀態會有所差異，不可能在每一個需要使用 userInfo 的 component 都寫一遍呼叫 api 的函數，所以無論是 init 的資料或是呼叫 api 的函數，這時候就會統一寫在 Vuex，方便要使用的人直接調用。