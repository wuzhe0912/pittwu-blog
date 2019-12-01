---
title: Git Command Error Fixed
date: 2019-12-01 15:24:16
tags: Git
---
`Git` 雖然是基本知識，但有時操作上，還是不免出現一些 error，記錄當中的解法，方便日後快速排查問題。
<!--more-->
## 初始化錯誤
由於之前習慣性從遠端 clone 下來，所以`.git`檔案同時會被準備完成，但這次因為嘗試自動部署`Hexo`，在推送時跳出下面這個錯誤。
```
fatal: Not a git repository (or any of the parent directories): .git
```
這個錯誤是說明，要推送的檔案不是一個`git`的`repository`，所以`git`在這個目錄底下會找不到`.git`檔案。解決方式是，執行指令`git init`，這樣就等於初始化一個`git repo`。