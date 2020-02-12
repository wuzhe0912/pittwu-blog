---
title: Vue-cli Init Step
date: 2020-02-12 20:13:27
tags: Vue.js
---
前端框架版本迭代太快，更新過程中安裝的版本又容易出現錯誤，單靠記憶不可行，只好記錄自己的操作步驟，隨時更新方便檢查。
<!--more-->
## 全域環境調整(vue-cli 4.0 以上版本)
### remove old version
```
npm uninstall vue-cli -g

# or

yarn global remove vue-cli
```
### install new version
```
npm install -g @vue/cli

# or

yarn global add @vue/cli
```
### check version
```
vue --version

# or

vue -V
```
## fixed yarn
### clean yarn cache
yarn 更新至 v1.19.0 後，一度在終端機一直跳提醒，google 到的解法是清除 cache。
但目前已是 v1.22.0 已無目前問題。
```
yarn update v1.19.0 後，需清除一次 cache，指令如下：

yarn cache clean
```
## build project
### version not match error
```
若出現vue的版本為 2.6.9，但套件採用 2.6.10 的 error

需升級vue的版本，指令如下：

yarn global add vue@2.6.11
```
## create
```
vue create project-name
```
### 選擇需要的項目
官方預設提供的 css 預處理器，存在一些配置上的錯誤，這邊不做使用
僅選擇 Router & Vuex(單元測試和 PWA 尚未掌握)
Router 採用 History Mode
### 進入項目並啟動
```
cd project
yarn server
```