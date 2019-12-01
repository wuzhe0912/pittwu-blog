---
title: Next Theme Error Fixed
date: 2019-11-29 23:04:06
tags:
  - Hexo
  - Next Theme
---
雖然`Hexo`的學習曲線平滑，網路上也是隨手可得教學資料，但仍免不了，在使用過程中碰到一些難點，因此順手記錄這些難點的解法，方便自己日後回顧檢查。
<!--more-->
## Next 主題背景動畫配置無效果
`Next`在5.x版本時，設定背景動畫效果，僅需在`_config.yml`調整`true or false`即有效果。但在6.X版本，不知道因為何種原因，這種設置方式完全失效，因此改採用下述三個步驟設置動畫效果。

e.g. 選擇 canvas_nest 這個效果
```
1. cd themes/next
2. git clone https://github.com/theme-next/theme-next-canvas-nest source / lib / canvas-nest
3. next/_config.yml 設定 enable: true

其他三種動畫效果，同前述。
```