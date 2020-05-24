---
title: Firebase 操作記錄筆記
date: 2020-02-12 22:14:32
tags: Firebase
---
記錄自己操作 Firebase 的流程，以及問題解法。
<!--more-->
## 新版 SDK 異動
除了引入的 firebase-app.js 是主要核心，其他功能則被拆分到各子項目。
若要調用 `firebase.database()` 需再引入以下 script：
```
<script src="https://www.gstatic.com/firebasejs/7.8.2/firebase-database.js">
</script>
```
### 檢查是否載入 Firebase 成功
```
const database = firebase.database()
console.log(database)
```
## 操作資料庫
`ref()` => 尋找資料庫路徑
`set()` => 新增資料
```
// 目前練習使用 Realtime Database
// 若沒有目錄的狀況，預設會寫入到根目錄
firebase.database().ref().set('Hello Firebase!')

// 除了字串，也可以寫入物件
firebase.database().ref().set({
  name: 'Pitt'
})
```
### 修改資料
```
// 原始資料
firebase.database().ref('firstArray').set([
  {
    name: 'Pitt',
    hasMac: true
  },
  {
    name: 'NiNi',
    hasMac: true
  },
])

// 尋找對應路徑後修改資料
firebase.database().ref('firstArray/0/hasMac').set(false)
// 陣列中的第一組變數 hasMac 即會修改為 false
```
### 顯示資料至頁面
`once()` => 讀取一次資料庫的資料
snapshot => 意指快照，其中 `snapshot.val()` 是 Firebase 的讀取語法，snapshot 本身是變數命名，可用其他名稱取代。
```
firebase.database().ref('userName').set('PittWu')
let userName = firebase.database().ref('userName')

userName.once('value', (snapshot) => {
  let item = snapshot.val()
  console.log(item)
  document.getElementById('title').textContent = item
})
```
### 隨時監聽資料
`on()` => 監聽
```
// 使用 on 語法時，當資料變更時，頁面也隨之渲染變化
firebase.database().ref('userName').set('Pitt')
let userName = firebase.database().ref('userName')
userName.on('value', (snapshot) => {
  let item = snapshot.val()
  document.getElementById('title').textContent = item
})
```
## Firebase 非同步
```
firebase.database().ref('userName').set('Pitt')
let userName = firebase.database().ref('userName')
userName.on('value', (snapshot) => {
  let item = snapshot.val()
  console.log('test1', item)
  document.getElementById('title').textContent = item
})
console.log('test2')
```
在前述程式碼中，正常資料庫回傳應該會需要等待時間，所以應該是先執行 `test2`，但在本地測試時，可能資料量太小，導致即使使用 Slow 3G，`test1` 依然先跑完，後續資料多的時候要再觀察非同步狀況。 
## 新增資料
透過 push 語法，添加資料至 Firebase，同時因為資料庫本身有協助建立 key 值，不需擔心資料衝突的問題。
```
const todoList = firebase.database().ref('todoList')
todoList.push({
  content: 'shopping'
})

// 會拿到如下格式的資料
-M00rdx2RilLDFK9lmSg {
  content: "shopping"
}
```
## 刪除資料
child => 子路徑
可以透過 `ref()` 找到根目錄，也可以透過 `child()` 找到子目錄，下面兩種做法功能相同：
```
let todoList = firebase.database().ref('todoList')
let todoList2 = firebase.database().ref().child('todoList')
```
remove => 刪除資料
最笨的刪除方法，直接根據 key 值進行刪除
```
todoList.child('-M00rdx2RilLDFK9lmSg').remove()
```
## 網頁即時查看 Firebase 資料變動
先透過 `on()` 語法監聽資料庫，將快照取得的資料渲染到頁面，同時為了方便查看，再使用 `JSON.stringify` 格式化，`null, 2`則是縮排便於查看。
```
const allData = firebase.database().ref()
allData.on('value', (snapshot) => {
  let path = document.getElementById('content')
  path.textContent = JSON.stringify(snapshot.val(), null, 2)
})
```
## 排序
透過 `orderByChild()` 選擇資料中排序的基準屬性，再透過 `forEach()` 依序撈出：
```
firebase.database().ref('practice').set(people)
let peopleRef = firebase.database().ref('practice')
// 先取得路徑 => 是否需要排序('屬性') => 讀取資料 => 使用 forEach 依序撈出
peopleRef.orderByChild('height').once('value', (snapshot) => {
  snapshot.forEach((node) => {
    console.log(node.val())
  })
})
```
### 排序規則
Firebase 的 `orderByChild()` 排序規則，需參考[官網文件](https://firebase.google.com/docs/database/admin/retrieve-data?hl=zh-cn#orderbychild)
## 搜尋區間
`startAt()` => 多少以上
`endAt()` => 多少以下
`equalTo()` => 等於
```
e.g. 體重3000以上
peopleRef.orderByChild('weight').startAt(3000).once('value', (snapshot) => {
  snapshot.forEach((node) => {
    console.log(node.val())
  })
})
```
```
// 也可以透過 startAt() 和 endAt() 交叉運用來設定範圍
peopleRef.orderByChild('weight').startAt(3000).endAt(5000).once('value', (snapshot) => {
  snapshot.forEach((node) => {
    console.log(node.val())
  })
})
```
```
// 相等於
peopleRef.orderByChild('weight').equalTo(3000).once('value', (snapshot) => {
  snapshot.forEach((node) => {
    console.log(node.val())
  })
})
```
## 限制筆數
`limitToFirst()` => 撈取從第一筆資料開始的資料，()內的條件設定預撈取的比數
`limitToLast()` => 反之，從最後一筆開始撈資料
```
peopleRef.orderByChild('weight').startAt(2000).limitToLast(2).once('value', (snapshot) => {
  snapshot.forEach((node) => {
    console.log(node.val())
  })
})
```
## 時間
先透過 `JS` 原生語法檢查時間：
```
let time = new Date()
console.log(time)
console.log(time.getFullYear())
// 在 JS 中，0 = 1月
console.log(time.getMonth())
// 週日(禮拜7) = 0、週一(禮拜一) = 1
console.log(time.getDay())
console.log(time.getHours())
console.log(time.getMinutes())
console.log(time.getSeconds())
// 1000毫秒 = 1秒
console.log(time.getMilliseconds())
```
### UNIX 時間
從協調世界時1970年1月1日0時0分0秒起算至今
```
let time = new Date()
time.getTime()  // 拿到總秒數
```
