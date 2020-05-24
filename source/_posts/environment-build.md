---
title: 工作環境搭建
date: 2020-05-21 09:55:18
tags:
  - 環境搭建
  - 工作
---
近期新工作取得新的 mac，整個環境初始化，需要耗費不少時間重新搭建需要的工具和安裝套件。為了避免日後同樣情況下，花費太多時間取得資源，因此記錄自己的搭建流程。
<!--more-->
## Browser
- install 主流瀏覽器
  - Chrome
  - Firefox
  - Safari(Mac 預設原生)

## 終端機環境與套件
### install HomeBrew
打開 terminal 輸入以下指令，安裝過程中，同步會安裝`xcode`。
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
檢查 homebrew 版本號，確認是否安裝正常
```
brew --version
```

### install iTerm2
使用 HomeBrew 進行安裝
```
brew cask install iterm2
```
後續預設使用 iterm 來執行指令

### install zash
若 zash 尚未安裝，則安裝 zash 來取代 bash
```
brew install zsh zsh-completions
```
調整預設的 shell 為 zsh
```

```

## IDE
