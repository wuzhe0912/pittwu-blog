---
title: Nuxt.js 實作 Blog
date: 2020-04-02 16:41:57
tags: Nuxt.js
---
寫在前面，為了理解 Nuxt 的基礎運用，動手實作一個簡易的 Blog，同時記錄下目前的流程，方便後續可以參照比對。
<!--more-->
受限於目前還未掌握 Node 開發後端的能力，因此直接採用官方建議的 CMS 系統(storyblok)。不過樣式就不特別處理，簡單的 RWD。另外渲染 markdown 的部分，v-html 要改的樣式太多，只能以後有時間再來慢慢處理。部署仍是萬年的 netlify 不變。

### Demo
- [Nuxt.js Blog 連結](https://activello-nuxt-blog.netlify.com/)

待優化的部分：
- API 呼叫等待時間，加入 Loading 樣式
- 關於 nuxt store 的部分還沒觸碰
- 專案複雜度太低，仍有許多潛在的雷點還未踩到
- nuxt 的基礎是 node，除了補強 node 知識，可能要考慮用 node 實作後端
- 高交互的頁面，可能會需要掌握多種 layout


### init
```
npx create-nuxt-app nuxt-blog

cd nuxt-blog
```
除了 axios 其他都採用預設默認。

### install plugin
```
// pug
yarn add pug pug-plain-loader

// scss
yarn add node-sass sass-loader
```
run 一下測試是否正常
```
yarn run dev
```

### 架構設計
- 共用樣式統一由 assets 控管。
- 盡可能多使用 html5 語義化標籤。

### Layout
- 因為功能相對較少，僅採用 default layout，主體拆成三個區塊，header、main(nuxt)、footer。
- HomePage 的文章列表抽成 component 形式。

### nuxt-link
- active 樣式使用官方預設的 class => .nuxt-link-exact-active

### Headless CMS
- 先試用 storyblok，未來再試試看 Strapi
- 本地安裝對應套件
```
yarn add storyblok-nuxt
```
- 引入格式：
```
modules: [
  ['storyblok-nuxt', { accessToken: 'xxxxxxxxx', cacheProvider: 'memory' }]
],
```

### asyncData()
在 MVC 架構下，網站的運作是由前端向後端發出請求，拿到頁面的 url 的同時，也由後端 return html 給前端。但到了 MVVM 架構下，router 改由前端處理，後端只負責給數據，頁面的渲染生成統一由前端負責，也因此爬蟲這種基於 server 端的技術，無法抓到頁面，也就無法達到 SEO 的效果。nuxt 中的 asyncData() 可以理解為在後端做處理，因為是在 server 端所以也不存在跨域問題。

### 動態 router
nuxt 的動態路由，需透過 _ 下底線來自動生成：
```
- pages
  - _postId

// => /:postId
```

### 解析 markdown 格式
```
yarn add marked
```
```
import marked from 'marked'

marked(res.data.story.content.content)
```

### v-html style
在 scoped 屬性下，v-html 無法直接修改樣式，但如果移除 scoped 可能會污染全域樣式，除了透過特殊命名外，還有一種方法可以修改樣式，就是透過 deep scoped，而且也不會影響到全域，寫法如下：
```
.post-content /deep/ pre {
  background-color: #f7f7f7;
}

.post-content /deep/ h2,h3 {
  margin: 8px 0;
}
```

### fetch API
同 asyncData，因為他們都是在組件初始化之前就已經執行的方法，這個時候 this 是無法指向我們需要的對象，會導致無法取得值，因此 fetch api 在 nuxt 中不可以使用 this。

### component name
組件名稱通常是方便我們使用 chrome 插件的 vue dev tools 時，可以快速查詢，但和 Vue 不同，nuxt 預設的 eslint 規則中，名稱首字必須大寫。

### Nuxt 生命週期
承前述，Nuxt 的 [lifecycle](https://zh.nuxtjs.org/guide/) 如下：
起步進入 => 
1. 檢查 Nuxt 專案內的 store(類似 Vuex)是否有需要執行的內容
2. 進入中間件(middleware)，並且做三件事，檢查全域設定(nuxt.config.js)，在檢查配對的 Layout，配對要進入的頁面
3. 驗證進入的頁面是否存在，是 => 進入該頁面，否 => 轉往 404 頁面
4. 初始化之前，先將 server端 資料取回
5. 開始渲染頁面

當點擊前往其他頁面時，重新回到第二步開始執行
