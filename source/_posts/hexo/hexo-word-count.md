---
title: Hexo 增加字數統計
date: 2020-06-01 16:01:13
tags:
  - Hexo
---
優化 Hexo Blog 呈現的形式，添加筆記字數統計與閱讀時間計算。
<!--more-->
## 安裝套件
```
yarn add hexo-symbols-count-time
```

## 調整 _config.yml 設定
在`_config.yml`底部添加以下設定：
```
symbols_count_time:
  # 文章是否顯示字數統計 & 閱讀時間
  symbols: true
  time: true
  # 網頁底部是否顯示字數統計 & 閱讀時間
  total_symbols: true
  total_time: true
```

## NexT 設定
預設已經開啟，所以無需額外調整。
```
symbols_count_time:
  separated_meta: true  # false 時只會顯示單行
  item_text_post: true  # 若為 false 只會顯示圖標和數字，不會顯示所需閱讀時間&文章字數
  item_text_total: true # footer 是否顯示 總字數文字與閱讀時間的文字
  awl: 4 # 計算字數
  wpm: 275 # 一分鐘閱讀的字數
```

重新啟動 Hexo，即可看到相關功能已經開啟。
