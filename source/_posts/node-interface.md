---
title: Node.js(建立後端API接口)
date: 2020-02-22 18:33:58
tags: Node.js
---
嘗試理解 `Node.js` 如何建立後端API接口，簡略記下自己的建立流程。
<!--more-->
## Install plugin
### POST
建立如註冊類的API時，需使用 `POST`，這邊安裝一個插件來應用。
```
yarn add body-parser
```
### 密碼加密
```
yarn add bcrypt
```
### avatar 大頭貼
```
yarn add gravatar
```
### jwt token
```
yarn add jsonwebtoken
```
### 驗證
```
yarn add passport passport-jwt
```