---
title: Python：實作爬蟲
date: 2020-06-16 23:31:47
tags:
  - Python
---
爬蟲是我一直想摸的東西，但礙於拖延症頻發，拖至今日才開始動工學習。而爬蟲爬到的資料，自然也要進行梳理彙整，產生一個自己看得順眼的報表，而這些工作則嘗試使用`Python`完成。
<!--more-->
## 什麼是爬蟲？
簡言之，透過連結上網站，並從網站上的某個頁面抓取我們需要的內容。既然要爬對方的資料，自然會模擬瀏覽器行為，抓頁面上的`DOM`結構資料。或者模仿使用者的行為，諸如`click`事件之類。當然，大量爬取資料也會本地資源的過度負荷，或是對方網站乘載量太差，而無法支撐。

## 爬蟲程式的環境
[下載 chrome_headless](https://sites.google.com/a/chromium.org/chromedriver/downloads)
下載完成後，將`chromedriver`放到對應的專案資料夾內。

### install Selenium
安裝自動化測試套件：
```
pip3 install selenium
```

### install BeautifulSoup
安裝解析網頁套件：
```
pip3 install beautifulSoup4
```