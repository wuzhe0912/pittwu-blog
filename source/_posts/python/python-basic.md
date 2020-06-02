---
title: Python 基礎知識
date: 2020-06-01 21:17:14
tags:
  - Python
---
記錄 Python 的基礎知識與操作。
<!--more-->
## install
### Python
- [官網下載網址](https://www.python.org/downloads/)
下載後解壓縮安裝，檢查是否安裝成功指令
```
python3

exit() // 退出
```

### Jupyter
```
pip3 install jupyter notebook
```

執行
```
jupyter notebook
```

## 變數
在`python`的環境中，宣告變數直接聲明即可：
```
player = 'Pitt'
print(player) // Pitt

player = 'Pitt'
Job = 'Witcher'
print(player + ' Job is ' + Job) // Pitt Job is Witcher
```
但需要注意不同型別的變數，不可以放在一起，需要轉型：
```
// error
player = 'Pitt'

Job = 'Witcher'
HP = 500

print(player + ' HP is ' + HP) // TypeError

// success
player = 'Pitt'

Job = 'Witcher'
HP = 500

print(player + ' HP is ' + str(HP)) // Pitt HP is 500
```

### 轉型方法
- 布林
```
one = 'variable'
two = ''

print(bool(one)) // true
print(bool(two)) // false
```

- 轉數字
```
number = '10'

print(float(number)) // 10.0
```

- 轉整數
```
number = 10.6

print(int(number)) // 10.0
```