---
title: 實作 Vue.js Web App 簡單工具
date: 2020-02-26 12:18:02
tags:
  - Vue.js
  - Web App
---
寫在前頭，這個 Demo 看似 Web App，但本質上並不是，畢竟我自己還沒有碰過 Hybrid App。但因為前職其中一項專案產品，其 Layout 佈局採用 Web App 的風格來實作，並且成功存活。因此自己也來試寫一個小型 Demo。

也因為沒有搭配後端，所以僅有接幾個 API 來呈現資料內容，並套用了 i18n 來處理多語系功能。
<!--more-->
### Demo
建議手機直接掃描操作進入網址。
![](/images/vue-tools-app.png)

- 結構：
  - 前端框架：Vue.js
  - UI：手刻(Flexbox)
    - Layout：App 風格

- 待優化：
  - 需要調整 UserStory
  - 加入會員系統
  - 另建一個專案來設定後台(有空的話...)

- Layout：
  和過往的 RWD 設計佈局不同，出現 footer vs navbar 的情境，在 RWD 的 footer，多數是不具備實質功能的，若非版權宣告，就是條列網站的一些聲明文字或連結。

  相反的 navbar 卻是重心之一，當使用者在手機或平板操作時，navbar 可以讓客戶快速切換自己想前往的頁面，而非上下滾動來尋找目錄。但這種 Web App 在 PC 的介面時，則會奇醜無比，目前尚未看到兼容兩者的方案。

### 開發流程
待重構優化後，重新撰寫~