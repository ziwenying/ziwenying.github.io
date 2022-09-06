---
title: 【觀念 06】CSS Box Model ( box-sizing )
date: 2022-09-06 10:09:03
tags:
  - [CSS]
categories:
  - [基礎觀念]
---

根據 CSS Box Model（盒模型）的定義，HTML 元素都可以看成是一個 Box。

<!-- more -->

一個 Box 會包含：content, margin, border, padding。
content 由 寬高所構成，而 margin, border, padding 的計算方式會根據 **box-sizing** 模式的種類而有所不同。

# content-box

box-sizing: content-box; 是預設的模式，會以元素的邊界為基準，往**外**長出 padding 和 border。
缺點：當一個元素需要設定 padding 或 border 時，原本的寬高需要扣除向外長的 padding 或 border，比較麻煩。

# border-box

box-sizing: border-box; 模式則會以元素的邊界為基準，往**內**長出 padding 和 border。
優點：不必每次都計算 padding 或 border，它們是向內扣的，比較直覺。

---

**參考資料：**

- [解釋 CSS Box Model ( box-sizing )](https://ithelp.ithome.com.tw/articles/10275847?sc=iThelpR)
