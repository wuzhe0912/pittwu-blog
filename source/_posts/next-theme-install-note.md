---
title: Next Theme Install Note
date: 2019-11-28 22:39:13
tags: Hexo
---
安裝`Hexo`的難度不高，但是調整`Theme`的配置是件不小的工程，記錄一下自己的調整流程。
<!--more-->

## 挑選主體
[挑選 Hexo 主題網址](https://hexo.io/themes/)，這個 Blog 以`Next`為例。
step1. 安裝`theme`
```
cd hexo-blog
git clone https://github.com/theme-next/hexo-theme-next themes/next
```
step2. 修改設定`_config.yml`
```
theme: landscape => theme: next
```
重新啟動`hexo s`
## 設定
Next 有四種 Scheme 可以選擇，預設主題風格是 Muse。
在`themes/next/_config.yml`中找到 scheme 設定，再將想選擇的註釋去除即可。
e.g.
```
# Schemes
# scheme: Muse
# scheme: Mist
# scheme: Pisces
scheme: Gemini
```
## 關於作者
### 新增大頭貼
step 1 建立存放圖片用的資料夾
```
mkdir source/images
```
step 2 將大頭貼的照片丟入資料夾`source/images`，接著在主題設定`themes/next/_config.yml`中，設定大頭貼路徑。
```
avatar:
  url: /images/avatar.jpeg
```
#### 調整大頭貼樣式
邊框改為圓形，設為 true
```
rounded: false => rounded: true
```
hover 時，添加旋轉特效
```
rotated: false => rotated: true
```
## 開啟社群帳號連結
在`themes/next/_config.yml`中可以打開或新增個人社群網站連結，僅須將註釋去除即可。
```
social:
  GitHub: https://github.com/wuzhe0912 || github
  E-Mail: mailto:kgb00128@gmail.com || envelope
```
## 文章預覽
在`themes/next/_config.yml`中，可以透過字數設定來才切預覽。
```
auto_excerpt:
  enable: false
  length: 150
```
也可以透過`<!--more-->`來裁切，在`<!--more-->`以上的文字，會出現在預覽。
## 右上角添加 Github Fork 圖片
step 1 首先到[GitHub Corners](http://tholman.com/github-corners/)或是[GitHub Ribbons](https://github.blog/2008-12-19-github-ribbons/)尋找自己喜歡的樣式，並 copy code
step 2 打開`themes/next/layout/_layout.swig`，在`<div class="headband"></div>`下面
step 3 將`href`後面的網址，替換成個人`GitHub`主頁。
重新啟動服務`hexo s`。
## 啟用文章閱讀進度條
打開`themes/next/_config.yml`，將`reading_progress`設為 true。
```
reading_progress:
  enable: true
```
## 左側啟用文章閱讀%數
同樣在`themes/next/_config.yml`中，調整下面參數，改為 true。
```
back2top:
  enable: true
  # Back to top in sidebar.
  sidebar: true
  # Scroll percent label in b2t button.
  scrollpercent: true
```