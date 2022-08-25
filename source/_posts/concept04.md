---
title: 【觀念 04】Scope 補充，以及什麼是 LHS 和 RHS？
date: 2022-08-25 11:09:18
tags:
  - [JavaScript]
categories:
  - [基礎觀念]
---

上一篇提到 Scope，這篇再筆記一下 Nested Scope，並介紹什麼是 LHS 和 RHS。

<!-- more -->

# Nested Scope

當 JavaScript 執行時，如果在目前所位於的 Scope 找不到需要的變數，這時就會往外面一層去尋找，假如還是找不到，就會再往外找，直到找到或是找到最外層 Global Scope 為止。
例子如下：

```
function theColor() {
  console.log(color)
}
var color = 'green'
theColor() // green
```

在 theColor function 裡面，並沒有 color 這個變數，因此就向外層找，找到了 var 變數，最後得到 green。

# 什麼是 LHS 和 RHS？

了解 LHS (Left Hand Side) 和 RHS ( Right Hand Side) ) 的目的是為了快速理解 JavaScript 報錯，並排除開發上發生的錯誤。

## LHS

Left Hand Side 是將值賦予左邊的變數上。
![image](https://github.com/ziwenying/ziwenying.github.io/blob/main/2022/08/25/concept04/article04-2.jpg?raw=true)

## RHS

Right Hand Side 是從右邊的變數上取值。
![image](https://github.com/ziwenying/ziwenying.github.io/blob/main/2022/08/25/concept04/article04-1.jpg?raw=true)

**補充：LHS 和 RHS 兩者混合**

```
var color = 'green'
// 這邊發生 LHS 賦值給變數 color

var color2 = color
// 發生變數 color 先透過 RHS 取值，再透過 LHS 賦值給變數 color2
```

## 當 JavaScript 報錯時

**※ 如果是 LHS，在 strict mode 就會拋出 ReferrenceError；假如是非 strict mode 就會在 Global Scope 建立這個找不到的變數。**

```
//情況一
"use strict"
color = 'green'
console.log(color) // ReferenceError: color is not defined
```

```
//情況二
color = 'green'
console.log(color) // 因為 color 變數被建立，所以才得到 green
```

**※ 如果是 RHS 的情況，也可能會拋出 ReferrenceError。**

```
function theColor() {
  console.log(color) // 發生 RHS
}
theColor()
// ReferenceError: color is not defined
```

# 補充：常見的 Javascript Error

- ReferenceError：當嘗試存取一個不存在的變數。
- SyntaxError：當語法輸入錯誤，或是使用 let 或 const 重複宣告變數。
- TypeError ：當一個值的類型非預期的那個類型時，就會發生。

---

**參考資料：**
[Summer - 你懂 JavaScript 嗎？#10 範疇（Scope）](https://cythilya.github.io/2018/10/17/what-is-scope/)
[錯誤種類](https://ithelp.ithome.com.tw/articles/10219168?sc=rss.iron)
[何謂 LHS、RHS 錯誤？](https://ithelp.ithome.com.tw/articles/10266176)
