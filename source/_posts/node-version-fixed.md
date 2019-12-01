---
title: Node & NPM Version Path Fixed
date: 2019-12-01 15:35:57
tags:
  - Node
  - NPM
  - NVM
---
近期公司 MIS 調整硬體設備，造成環境有點跑掉，記錄一下自己在 stackoverflow 上找到的解法。
<!--more-->
打開 iTerm 會出現下述 error。
```
nvm is not compatible with the npm config “prefix” option: currently set to “/Users/xxx/.nvm/versions/node/v8.12.0"
Run `npm config delete prefix` or `nvm use --delete-prefix v8.12.0 --silent` to unset it.
```
從字面上來看，應該是 npm 和 nvm 管理的 node 版本沒有對上，按照終端機提供的訊息，敲入對應指令。再檢查 node 版本似乎是正常了，但事實上，若在 iTerm 上另開分頁，依然會跳相同的提示錯誤。雖然不影響操作，但看到總是不順眼，google 了一下解法，最終測試成功方案如下：
```
npm config delete prefix
npm config set prefix $NVM_DIR/versions/node/v8.12.0
```
看起來應該是先刪除 npm 中設定的 prefix ，再重新設定當前 nvm 使用的版本。

最後附上 [stackoverflow](https://stackoverflow.com/questions/34718528/nvm-is-not-compatible-with-the-npm-config-prefix-option) 找到的解法。