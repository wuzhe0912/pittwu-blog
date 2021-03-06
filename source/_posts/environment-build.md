---
title: 工作環境搭建(Mac)
date: 2020-05-21 09:55:18
tags:
  - 環境搭建
---

近期新工作取得新的 mac，整個環境初始化，需要耗費不少時間重新搭建需要的工具和安裝套件。為了避免日後同樣情況下，花費太多時間取得資源，因此建立一個 check
list，方便自己快速比對確認。

<!--more-->

## Browser(瀏覽器)

安裝主流瀏覽器

1. Chrome
2. Firefox
3. Safari(Mac 預設原生，無需安裝)

## 終端機環境與套件

### HomeBrew

打開 terminal 輸入以下指令，安裝過程中，同步會安裝 xcode。

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

檢查 HomeBrew 版本號，確認是否安裝正常

```
brew --version
```

### iTerm2

使用 HomeBrew 進行安裝

```
brew cask install iTerm2
```

後續預設使用 iTerm2 來執行指令

### zash

若 zash 尚未安裝，則安裝 zash 來取代 bash

```
brew install zsh zsh-completions
```

調整預設的 shell 為 zsh

```
sudo sh -c "echo $(which zsh) >> /etc/shells"
chsh -s $(which zsh)
```

重新啟動 iTerm2，使用指令檢查是否設定成功

```
echo $SHELL
```

### oh-my-zash

oh-my-zash 可以理解為 zash 的框架，安裝的目的是為了使用其外掛與主題(theme)，安裝指令：

```
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

打開`.zshrc`更改主題

```
open ~/.zshrc
```

找到`ZSH_THEME`後，可以修改為自己喜歡的 [theme](https://github.com/ohmyzsh/ohmyzsh/wiki/themes)，預設是 robbyrussell，可改為自己喜歡的：

```
ZSH_THEME="robbyrussell"
```

如果字體出現亂碼狀況，可以下載對應字體，譬如使用 agnoster theme，則可以下載`Melso`。字體修改路徑：

```
Profiles > Open Profiles > Edit Profiles... > Text > Font
```

### zash 權限問題
如果運行終端機的環境時出現以下狀況：
```
[oh-my-zsh] For safety, we will not load completions from these directories until
[oh-my-zsh] you fix their permissions and ownership and restart zsh.
[oh-my-zsh] See the above list for directories with group or other writability.

[oh-my-zsh] To fix your permissions you can do so by disabling
[oh-my-zsh] the write permission of "group" and "others" and making sure that the
[oh-my-zsh] owner of these directories is either root or your current user.
[oh-my-zsh] The following command may help:
[oh-my-zsh]     compaudit | xargs chmod g-w,o-w

[oh-my-zsh] If the above didn't help or you want to skip the verification of
[oh-my-zsh] insecure directories you can set the variable ZSH_DISABLE_COMPFIX to
[oh-my-zsh] "true" before oh-my-zsh is sourced in your zshrc file.
```
代表 oh-my-zash 需要修復權限，打開文件：
```
open ~/.zshrc
```
文件中加入這一行
```
ZSH_DISABLE_COMPFIX=true
```
重新運行
```
source ~/.zshrc
```

### Reference

[透過在 MAC 上安裝 iTerm2 活潑你的終端機](https://dustinhsiao21.com/2019/04/09/%E9%80%8F%E9%81%8E%E5%9C%A8-mac-%E4%B8%8A%E5%AE%89%E8%A3%9DiTerm2-%E6%B4%BB%E6%BD%91%E4%BD%A0%E7%9A%84%E7%B5%82%E7%AB%AF%E6%A9%9F/)

## 解壓縮工具

## IDE

2020 年的現在，仍以 Visual Studio Code 為主。

### install

[官網下載網址](https://code.visualstudio.com/Download)

### 擴充套件

- Beautify
- Bracket Pair Colorizer
- Code Spell Checker
- ESLint
- Git History
- GitLens
- Vue 2 Snippets
- Vue VSCode Snippets
- Vetur
- Color Highlight
- Material Icon Theme
- Auto Close Tag
- Auto Rename Tag
- Path Intellisense
- Path Autocomplete
- indent-rainbow
- One Dark Pro
- One
- Markdown Preview Enhanced

### Theme
- Material Theme
  - 採用 default

## Git

### install

使用 HomeBrew 的指令安裝 Git

```
brew install git
```

### GUI

[Sourcetree 官方網站](https://www.sourcetreeapp.com/)

## 工具

- Slack
- Postman
- Zeplin
- iStat Menus

### 依各自公司需求

- Rocket.Chat

## NVM

使用 NVM 工具來管理 Node 版本與環境，安裝指令：

```
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash
```

重新啟動 iTerm2 後，檢查 nvm 是否安裝成功

```
nvm --version
```

查詢遠端可安裝的 Node 版本指令：

```
nvm ls-remote
```

安裝所需要的 Node 版本號(通常安裝 LTS version)：

```
nvm install v12.16.3
```

切換所需使用的 node 版本

```
nvm use v12.16.3
```

設定預設使用的 node 版本

```
nvm alias default v12.16.2
```

檢查是否正常安裝成功

```
node -v
npm -v
```

## Yarn

安裝 Facebook 提供的模組管理工具 Yarn，雖然官方推薦使用 HomeBrew 來安裝，但我自己安裝的過程中是存在問題的，查了相關的 issues，多是推薦改用備選方案的指
令進行安裝：

```
curl -o- -L https://yarnpkg.com/install.sh | bash
```

## VSCode 加入終端機指令
打開 VSCode 後開啟快捷鍵
```
shift + cmd + p
```
輸入
```
shell command
```
選擇
```
Shell Command: Install code command in PATH
```
重新啟動後，即可在終端機使用`code`打開 VSCode。
