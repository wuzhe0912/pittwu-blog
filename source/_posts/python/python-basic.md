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

### 練習
建立一個變數 num，該 num 的意義為儲存『答對題數』，透過這個 num 去計算出對應的得分，並存在變數 ans 中，然後印出確定符合記分規則。規則如下：
1. 答對題數在 0~10 者，每題給 8 分。
2. 題數在 11~20 者，從第 11 題開始，每題給2分。(前 10 題還是每題給 8 分)
3. 題數在 20 以上者，一律 100 分。

解法：
```
# num 為答對題數
num = 16
# ans 為加總分數
ans = 0

if (num <= 10):
  ans = str(num * 8)
  print('你的分數：' + ans)
elif (num >= 11 and num <= 20):
  lessNum = (num - 10) * 2
  ans = str(10 * 8 + lessNum)
  print('你的分數：' + ans)
else:
  print('你的分數：100')
```

## 迴圈
### while
```
num = 1
while( num < 5):
    print(num)
    num = num + 1

// 印出
1
2
3
4
```

### for
for 迴圈的寫法更為簡便直覺：
```
for item in range(1, 5):
    print(item)

// 印出
1
2
3
4
```

### 雙重迴圈
```
for item in range(1, 3):
    for subItem in range(1, 3):
        print(subItem)
```

### 練習：99 乘法表
```
for node in range(1, 10):
    for subNode in range(1, 10):
        print(str(node) + '*' + str(subNode) + '=' + str(node * subNode))
```

### 練習：迴圈結合 if/else
輸入 A001 到 A100，範例如下：
A001
A002
A003
...
...
A100

解法：
```
for item in range(1, 101):
    if(item < 10):
        print('A00' + str(item))
    elif(item >= 10 and item <= 99):
        print('A0' + str(item))
    else:
        print('A' + str(item))
```

## 資料操作
`array`和`JS`的操作方法幾乎雷同，使用方法相去不遠：
```
studentList = ['Mui', 'Kui', 'Mini']

print(studentList) // ['Mui', 'Judy', 'Mini']
print(studentList[1]) // Judy
```

### 倒數
```
print(studentList[-1]) // Mini
```

### 從目錄開始數幾個
```
print(studentList[0:2]) // ['Mui', 'Kui']
```

### Tuple
和`array`相類似，但建立後，裡面的資料不可再改變，使用`()`包裹資料：
```
drinks = ('Milk', 'Black Tea', 'Milk Tea', 'Soy Milk')

print(drinks) // ('Milk', 'Black Tea', 'Milk Tea', 'Soy Milk')
print(drinks[3]) // Soy Milk
drinks[1] = 'Green Tea' // TypeError 不可改變
```

### dict
`enumerate(迭代)`方法跑過整個列表
```
studentList = ['Tania', 'Neisa', 'Sabrina', 'Yennefer']

print(len(studentList)) // 4，計算陣列長度

listLength = len(studentList)
for item in range(0, listLength):
    print(studentList[item])
```

## def(函式)
函式的基礎寫法：
```
// 印出
def getNumber():
    number = 10
    print(number)

getNumber() // 印出 10
```
```
// return 回傳
def getNumber():
    number = 20

    return number

getNumber()
```
傳值：
```
def getNewNumber(val):
    print(val) // 印出 17
    newVal = val + 120
    return newVal // return 137

getNewNumber(17) // 137
```

## 練習
建立一個`Array`，內有不等數字，傳入函式中，經由迴圈判斷陣列中每一個數字，若為奇數則印出歐巴，反之則印出歐膩：
```
numListOne = [2, 5, 11, 50, 34, 16, 71, 100, 46, 22, 33]
numListTwo = [2, 10, 12, 11, 33, 77]

def countNumber(val):
    for item in range(0, len(val)):
        if( val[item] % 2 == 0 ):
            print('歐巴')
        else:
            print('歐膩')

countNumber(numListOne)
countNumber(numListTwo)
```

### 另一種寫法
```
numListTwo = [2, 10, 12, 11, 33, 77]

def countNumber(val):
    for item in val:
        print(item)
        if( item % 2 == 0 ):
            print('歐巴')
        else:
            print('歐膩')

countNumber(numListTwo)
```