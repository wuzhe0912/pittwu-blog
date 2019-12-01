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
建立存放圖片用的資料夾
```
mkdir source/images
```
將大頭貼的照片丟入資料夾`source/images`，接著在主題設定`themes/next/_config.yml`中，設定大頭貼路徑。
```
avatar:
  url: /images/avatar.jpeg
```