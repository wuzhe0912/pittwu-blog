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
- [官網下載網址](https://www.Python.org/downloads/)
下載後解壓縮安裝，檢查是否安裝成功指令
```
Python3

exit() // 退出
```

### Jupyter
```
pip3 install jupyter notebook
```

執行指令
```
jupyter notebook
```

`jupyter`環境開啟後，點選右上角的`new`開啟新專案。


## 變數
在`Python`的環境中，宣告變數直接聲明即可：
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

### print()
在`Python`的環境中，`print()`類似於 JS 的`console.log`。

## 運算符號
加減乘除
```
num = 8
print(num) // 印出 8

print(num + 4) // 印出 12

print(num - 3) // 印出 5

print(num * 6) // 印出 48

print(num / 2) // 印出 4.0
```
### 相等
不同於`JS`的語法特性，需要因為嚴謹性而使用`===`，`Python`僅需要使用`==`：
```
print(num == 8) // 印出 true
```

### 多重判斷寫法
不同於`JS`使用`&&、||`，`Python`使用更為語意直覺的英文`and、or`：
```
print(num <= 10 and num > 6) // 印出 true

print(num > 12 or num < 4) // 印出 false

print(num > 6 or num < 4) // 印出 true
```

## if/else
寫法如下：
```
num = 10

if (num > 5):
    print('Hello')
else:
    print('World')

// 印出 Hello
```
不同於`JS`使用`{}`包裹，`Python`使用縮排4格來判斷。

### elif(else if 的縮寫)
```
num2 = 7
if (num2 > 10 and num2 < 3):
    print('Kimi')
elif (num2 >= 7 or num2 <= 3):
    print('Komi')

// 印出 Komi
```