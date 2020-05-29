---
title: 前端送出資料進行加密
date: 2020-05-28 15:56:02
tags:
  - JSEncrypt
---
前端送出資料時(例如註冊、登入)，基於安全性考量，應該進行加密而非明文傳輸，目前已知的做法是採用`JSEncrypt`套件進行加密，再由後端進行解密，這邊記錄目前已知的做法。
<!--more-->
## install
```
yarn add jsencrypt

# or

npm install jsencrypt
```

## 設置公鑰
公鑰為後端所提供，可能提供的方式有二，一是後端提供一組寫死的固定公鑰，二是由後端出API，提供動態的公鑰，這邊的做法先記錄第一種。另外，需要注意的是，若是前端需要進行解密動作，則後端同時也要提供私鑰。
```
// Vuex store
const state = {
  publicKey: `
  -----BEGIN PUBLIC KEY-----
  xxx/xxxx/xxx
  -----END PUBLIC KEY-----`
}

const getters = {
  ['publicKey'](state) {
    return state.publicKey;
  }
}
```

## 建立加密函式
```
// src/utils/index.js

import JSEncrypt from 'jsencrypt';
import store from '../store';

const encrypted = (userInfo = '') => {
  const encrypt = new JSEncrypt();
  encrypt.setPublicKey(store.getters.publicKey);
  return encrypt.encrypt(JSON.stringify(userInfo));
};

export {
  encrypted
};
```

## import component
將要送出的參數抽離到物件中
```
// login
const userInfo = {
  name: this.name,
  passWord: this.pwd
};
api
  .post('eLogin', userInfo)
```

## axios 攔截器進行加密
```
// src/api/index.js
import { encrypted } from '../utils';

axios.interceptors.request.use(
  config => {
    // 這邊可以加入所有需加密的API，例如login、register
    const encryptedUrL = ['eLogin'];
    let haveEncryptedUrL = encryptedUrL.some(j => j === config.url);
    haveEncryptedUrL && (config.data = encrypted(config.data));
    return config;
  },
  error => {
    return Promise.reject(error);
  }
);
```
