---
title: Vue.js & Firebase 實作即時聊天室
date: 2020-02-26 14:59:58
tags:
  - Vue.js
  - Firebase
---
首次嘗試套用 Vue.js + Firebase 來實作一個聊天室 Demo，但因為沒有透過後端轉發，安全性上大概是0吧(苦笑)。考慮到後續還要摸 Nuxt.js，學習 Node.js 這一塊是躲不掉了。
<!--more-->
### Demo
- [Vue Chat 連結](https://vue-chat-6a66d.firebaseapp.com/)

- 結構：
  - 前端框架：Vue.js
  - UI：手刻(Flexbox)
    - Layout：App 風格
  - Database：Firebase

### 預期重構
#### 優化
- 聊天室輸入內容時，可以添加檔案或圖片
- 添加標情符號欄位
- 玩家可以自創私人聊天室(類似群組)

#### 修正
- 在多人註冊的狀況下，聊天內容會產生傳送誤判，需要檢查資料結構，若無解的話，可能需要採用 socket.io


### 開發流程
此處內容待重寫ing...
