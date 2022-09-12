---
title:【觀念 05】function declaration 和 function expression 之間的差別？
date: 2022-08-27 13:53:41
tags:
  - [JavaScript]
categories:
  - [基礎觀念]
---

function declaration 和 function expression 直接來看程式碼會比較清楚它們的差異。

<!-- more -->

# function declaration（函式聲明式）

```
theFun() // This is a function.
function theFun() {
  console.log('This is a function.')
}
```

因為 JavaScript 有 hoisting，所以 theFun 這個函式在 JavaScript 真正開始執行前，就已經被整個宣告。等到程式碼正式執行時，自然就可以被呼叫。
而當 theFun 被宣告後，是無法中途被回收的，會一直存於記憶體之中。

# function expression（函式表達式）

```
theFun() // ReferenceError: Cannot access 'theFun' before initialization
const theFun = function() {
  console.log('This is a function.')
}
也可以寫成 arrow function：
const theFun = () => {
  console.log('This is a function.')
}

theFun() // This is a function.
```

第一個 theFun() 之所以會出現錯誤，是因為 theFun 這個變數雖然被 hoisting 並宣告了，但後面的 function() 並未被 hoisting，才會出現 ReferenceError。
當 theFun 被宣告後，以 expression 呈現的 function 是可以隨變數的週期進行迭代或是被回收。

# function declaration 和 function expression 的差異

1. function declaration 可以在函式前呼叫，function expression 必須在函式後呼叫。
2. function declaration 被宣告後就會一直存於記憶體中；function expression 可以隨變數的週期更迭，或是被回收。

---

**參考資料：**

- [聲明式( declaration ) vs 陳述式 ( expression )](https://medium.com/@unick.zhow/%E8%81%B2%E6%98%8E%E5%BC%8F-declaration-vs-%E9%99%B3%E8%BF%B0%E5%BC%8F-expression-b9e62e385484)
