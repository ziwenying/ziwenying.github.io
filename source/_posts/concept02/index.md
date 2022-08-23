---
title: 【觀念 02】了解提升 (hoisting)
date: 2022-08-23 09:39:44
tags:
  - [JavaScript]
categories:
  - [基礎觀念]
---

一開始學習程式的時候，有看到一個觀念是 **JavaScript 程式是由上而下，一行接著一行跑的**，所以當時就認為函式和變數應該都要先宣告之後才可以使用。

<!-- more -->

# 為什麼沒有宣告卻可以使用？

實際上使用 JavaScript 的時候卻發現跟想像中的不太一樣，讓我曾經困惑過，為什麼 function 明明是後面才進行宣告，卻已經可以進行呼叫？
同時還有另外一個困惑，為什麼在 JavaScript 中，console.log() 一個尚未宣告的變數，卻得到 undefined，而非得到 ReferenceError: Cannot access '變數' before initialization。

經過了解之後，才知道原來是被 hoisting 了。

```
console.log(num) // undefined
var num = 1
```

# 為什麼會 hoisting？

因為 JavaScript 編譯器最初會先找出程式碼中的所有變數，並綁定 scope，但並不會賦值。
所以要特別注意的是，只有宣告的變數會被 hoisting，賦值並不會一起被 hoisting，所以上面的例子才會得到 undefined。
至於賦值的部分要等到 JavaScript 引擎開始執行時，才會賦值給變數。

那，前面提到 function 的部分呢？明明是後面才進行宣告，卻已經可以進行呼叫，這也是因為 hoisting 的緣故。
假如變數和 function 宣告為相同的名字呢？
來看看下面的例子：

```
console.log(num) //會得到 function num
var num function num(){}
```

這是因為 hoisting 的過程中，**function 宣告會優先於變數的宣告**。

# hoisting 的好處

hoisting 的存在不是沒有理由的，它的好處是可以讓 function 互相呼叫。
假如沒有 hoisting，就只能使用後面的 function 去呼叫前面的 function，會變得很不方便。

自從 JavaScript ES6 的 let 和 const 的用法出現後， 就比較少使用 var，但下面這個範例得到 Cannot access 'b' before initialization。這是怎麼回事？

```
console.log(b) //ReferenceError: Cannot access 'b' before initialization
let b = 5
```

# 難道 let 和 const 不會有 hoisting 嗎？

答案其實是有的。

var 宣告的變數在一開始會被初始化成 undefined，如果使用的是 let 和 const 則不會被初始化成 undefined，所以才會在取值的時候發生錯誤。
所以不管是 var、let 或 const 都會發生 hoisting。

---

**參考資料：**

- [Summer。桑莫。夏天 - 你懂 JavaScript 嗎？#13 拉升（Hoisting）](https://ithelp.ithome.com.tw/articles/10203357)
- [我知道你懂 hoisting，可是你了解到多深？](https://github.com/aszx87410/blog/issues/34)
