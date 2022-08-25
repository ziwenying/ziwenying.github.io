---
title: 【觀念 03】JavaScript 的作用域 (scope)
date: 2022-08-24 17:04:19
tags:
  - [JavaScript]
categories:
  - [基礎觀念]
---

# Scope（作用域，或譯作範疇）是什麼？

可以看作是一個變數的的生存範圍，當離開這個範圍，就無法存取到這個變數。

**JavaScript 的作用域分為三種：**

1. Global Scope
2. Function Scope
3. Block Scope (ES6)

<!-- more -->

在發布 ES6 之前，僅有 Global Scope 和 Function Scope 兩種。

# 全域變數 (Global Variables)

## Global Scope

在 JavaScript 的執行環境中，會有一個 Global Object，又稱全域物件。
如果是在瀏覽器的環境下，Global Object 是 window object。
如果是在 Node.js 的環境下，Global Object 是 global object。

只要是在 Global Object 中宣告的變數，在整個程式內都可以被取來使用，稱為全域變數 (Global Variables)。
例如我們宣告了 var word = 'word A'，如下：

```
function theFunc() {
  console.log('in function', word)
}

var word = 'word A'
console.log('in global', word) // in global word A
theFunc() // in function word A
```

不論是在 Global 還是在 Function 中都可以取用到 var word。

# Local Variables（區域變數）

## Function Scope

有些變數的作用域是只存在 function 內部，例子如下：

```
function theFunc() {
  var word = 'word B'
  console.log('in function', word)
}

console.log('in global', word) // ReferenceError: word is not defined
theFunc() // in function word B
```

變數 word 只要在 function 外呼叫，就會出現錯誤。

## Block Scope

這個是 JavaScript 中範圍最小的作用域，Block 指的是程式碼中用 {} 包起來的區域。這是 ES6 新增的特性，因此要使用 let 或 const 才有效果。
用個簡單的 for 迴圈來舉例：

```
for (let i = 0; i < 1; i++) {
  let j = 1
  console.log('j in block', i) //i in block 1
}
console.log('j', i) // ReferenceError: j is not defined
```

如果取用的地方在 {} 之外，就會出現 ReferenceError。

# Global Variables 和 Local Variables

介紹了作用域，來簡單比較一下 Global Variables 和 Local Variables。
以影響的廣度來看，肯定是 Global Variables 比較廣，在整體程式的地方都可以取用變數，但如果取用的位置是區域性的，論影響力（優先度）就是 Local Variables 為優先。
沿用上面的例子，修改後如下：

```
function theFunc() {
  var word = 'word B'
  console.log('in function', word)
}

var word = 'word A'
theFunc() // in function word B
```

呼叫 Function theFunc 會得到 in function word B，因為在 function 內取用的話，就會以 function 自己內部宣告的變數為優先。

# ※ 補充 - 宣告變數的方式

**var 的缺點：**

1. 允許同變數名稱重複宣告。
2. var 不會有 Block Scope 的效果。
3. 缺乏常數的特性（同變數可以重複被賦值）。

**let 和 const 的優點：**

1. const 宣告的變數，後續不能再重新賦值。
2. 禁止重複宣告變數，舉例如下：
   不論前面是先用 var、let 還是 const 宣告都一樣。

```
var word = 'apple'
let word = 'orange'
出錯： SyntaxError: Identifier 'word' has already been declared

let color = 'red'
let color = 'yellow'
出錯： SyntaxError: Identifier 'color' has already been declared
```

- 可以留意的是，一個 block 內部的子 block 是可以重複宣告同名稱的變數。

---

參考資料：
[變數的作用域(Scope)](https://ithelp.ithome.com.tw/articles/10203387)
[Huli - 所有的函式都是閉包：談 JS 中的作用域與 Closure](https://blog.huli.tw/2018/12/08/javascript-closure/)
