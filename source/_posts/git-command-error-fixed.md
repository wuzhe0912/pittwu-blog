---
title: Git Command Error Fixed
date: 2019-12-01 15:24:16
tags: Git
---
Git 雖然是基本知識，但有時操作上，還是不免出現一些 Error，記錄當中的解法，方便日後快速排查問題。
<!--more-->
## 初始化錯誤
由於之前習慣性從遠端 clone 下來，所以`.git`檔案同時會被準備完成，但這次因為嘗試自動部署`Hexo`，在推送時跳出下面這個錯誤。
```
fatal: Not a git repository (or any of the parent directories): .git
```
這個錯誤是說明，要推送的檔案不是一個`git`的`repository`，所以`git`在這個目錄底下會找不到`.git`檔案。解決方式是，執行指令`git init`，這樣就等於初始化一個`git repo`。
## GitLab 推送失敗錯誤
這邊在 push branch 到 GitLab 時遇到一個問題，狀況如下：
```
GitLab: The project you were looking for could not be found.
fatal: Could not read from remote repository.
```
遠端的 GitLab 找不到我的 repository 位置，這邊會出現兩種可能，第一是還沒加入 SSH Key，第二是 SSH Key 正確，但需要重新更換遠端的名稱，首先輸入下面的指令，檢查本機公鑰有沒有正常加入 GitLab：
```
ssh -T git@gitlab.com
```
如果出現 `Permission Denied (publickey)` 那就需要執行生成 SSH Key，並將公鑰加入 GitLab。
1. 生成 SSH Key：
```
ssh-keygen -t rsa -C "YOUR EMAIL"

cd ~/.ssh/

// 注意生成過程中，會要求設定私人密碼，需要自己記住
```
2. 進入 ssh 底下，會看到兩個新的檔案：
```
id_rsa => 私鑰
id_rsa.pub => 公鑰 // 使用 vscode 將公鑰的檔案打開，複製裡面的內容
```
3. 回到 GitLab，操作路徑：
```
點選右上角圖像/setting/左側 SSH Keys

// 將剛剛複製的內容貼到中間的輸入框，並點擊下方的 add key
```
4. 回到終端機，再次輸入`ssh -T git@gitlab.com`，這時會要求你輸入剛剛設定的私人密碼，成功後應該會看下面的文字：
```
Welcome to GitLab, @Your_name!
```
到這邊第一步設定 SSH Keys 完成，但我嘗試重新 push 一次依然失敗：
```
remote: The project you were looking for could not be found.
fatal: repository 'https://gitlab.com/Your_name/Your_project.git/' not found
```
5. 重新更新並配對一次遠端名稱：
```
git remote rename origin old-origin
git remote add origin git@gitlab.com:Your_name/Your_project.git
```
到此，檔案就能正常推送到遠端了。