---
title: Hexo 設置 Google Search Console
date: 2020-06-03 00:09:38
tags:
  - Hexo
---
搭建了技術Blog，雖然筆記內容粗淺，但私心仍會期望能讓更多人看到，為了提升搜尋引擎的曝光度，自然要將Blog網址提交到 Google Search Console。
<!--more-->
## install
```
yarn add hexo-generator-sitemap
```
## 設定 sitemap 路徑
打開跟目錄底下的`_config.yml`
```
#Sitemap
sitemap:
  path: sitemap.xml
```