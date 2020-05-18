---
title: 面試問題：Vue.js - II
date: 2019-6-8 08:14:29
tags:
  - Interview
  - Vue.js
---
Vue.js 相關面試問題彙整 II。
<!--more-->
### 聊一下，你所知道的 Vue 組件通訊方式中，較少見的做法。
Vue 的父子組件中，還有一種不特別推薦的通訊方式，父組件通過 $children 可以改變子組件的 data，相反的，子組件可以透過 $parent 來改變父組件的 data。但官方建議少用這種方式，應該是要避免數據流動被搞混。