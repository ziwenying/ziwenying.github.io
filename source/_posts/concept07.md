---
title: 【觀念 07】紀錄一些 SASS、SCSS 基本用法
date: 2022-09-09 09:34:39
tags:
  - [SCSS]
categories:
  - [基礎觀念]
---

# SASS/SCSS

Sass (Syntactically Awesome Stylesheets) 在發展的過程中，衍生出兩種語法，所以蠻常會看到 Sass/SCSS，分別為兩種副檔名 .scss 和 .sass。

<!-- more -->

副檔名 .scss 是目前主流使用，副檔名 .sass 則是較舊的版本，在一般情況下會通稱為 Sass。

**兩者比較明顯的差異在於：**

1. SASS 把 {} 和 ; 都省略了，排版上要特別注意。

2. SASS 不像 SCSS 也可以使用原生 CSS。

# SCSS 常見用法：

1. 具有巢狀結構。

```
.outer {
  width: 10px;
  .inner {
    width: 20%;
  }
}
```

2. 可以使用 $ 宣告變數。

```
$white : #fff;　

div {　　　
color : $white; // #fff
}
```

3. & 可以代表父元素。

```
.btn {
    font-size: 16px;
    &:hover {
      box-shadow: 1px 1px 5px $white;
    }
  }
```

4. 使用 #{} 將變數嵌入字串中。

```
$side : right;
→ 使用：
.rounded {　　　　
margin-#{$side}: 5px;　　
}
```

5. 可以使用 + - \* /。

```
.box {　　　　
margin: (20px/2);　// 10px
}
```

6. % 宣告類別。

```
%btn-style {
  width: 20px;
  color: $red;
}
使用>>
.btn {
  @extend %btn-style ;
}
```

7. 使用 @mixin 進行宣告。

```
$num: 100px;
@mixin userAvatar($num) {
  width: #{$num}px;
  height: 50px;
  border-radius: 50%;
}
使用>>
.step {
  @include userAvatar(可放變數);
}
```

---

**參考資料：**

- [[CSS] Sass 入門教學-新手上路重點摘要)](https://ithelp.ithome.com.tw/articles/10244301)
