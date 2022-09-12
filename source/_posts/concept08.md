---
title: 【觀念 08】JavaScript 的 this 綁定方式
date: 2022-09-12 10:51:09
tags:
  - [JavaScript]
categories:
  - [基礎觀念]
---

當執行某個函式時，JavaScript 的關鍵字 this 可以指向調用此函式的「物件」，也可以看成，當調用某個函式時，this 會被自動添加進此函式中。

<!-- more -->

this 綁定方式這邊介紹常見的四種：

# 隱式綁定 (implicit binding)

當函式執行時，是「誰」呼叫此函式，this 通常就會自動指向那個「誰」。
下面看個範例：

```
let novel = {
  name: 'ABC',
  about: 'family',
  impressions : 'It is funny.',
  introduction: function() {
    console.log(`The novel ${this.name} is about ${this.about}. ${this.impressions}`)
  }
}
novel.introduction() // 印出 The novel ABC is about family. It is funny.
```

novel 物件中呼叫了函式，因此函式中的 this 會指向 novel 物件。

# 顯式綁定 (explicit binding)

不讓程式自動綁定，而是使用 .call()、.apply()、.bind() 來綁定想要綁定的東西。

1. call()：this 會跟「代入 .call()的參數」綁定。以下例子：呼叫 introduction()。

```
const introduction = function (cover) {
  console.log(`The novel ${this.name} is about ${this.about}. ${this.impressions} Cover: ${cover}`)
  console.log('this', this) //obj novelABC
}

let novelABC = {
  name: 'ABC',
  about: 'family',
  impressions: 'It is funny.',
}

introduction.call(novelABC, 'yes')
// The novel ABC is about family. It is funny. It is cool! Cover: yes
```

2. apply()：用法與 .call() 差不多，不同點在於參數使用陣列的方式代入。

```
const introduction2 = function (inpressions2, cover) {
  console.log(`The novel ${this.name} is about ${this.about}. ${this.impressions} ${inpressions2} Cover: ${cover}`)
  console.log('this', this) //obj novelABC
}

let novelABC = {
  name: 'ABC',
  about: 'family',
  impressions: 'It is funny.',
}

introduction2.apply(novelABC, ['It is cool!', 'yes'])
// The novel ABC is about family. It is funny. It is cool! Cover: yes
```

3. bind() 與 .call() 和 .apply() 最大的差別在於—— .call() 和 .apply() 會回傳函式執行結果，而 .bind() 是會回傳綁定的函式，不會自動執行，需要額外呼叫，例如：

```
const introduction2 = function (inpressions2, cover) {
  console.log(`The novel ${this.name} is about ${this.about}. ${this.impressions} ${inpressions2} Cover: ${cover}`)
  console.log('this', this) //obj novelABC
}

let novelABC = {
  name: 'ABC',
  about: 'family',
  impressions: 'It is funny.',
}

console.log(introduction2.bind(novelABC))
// 印出 function (inpressions2, cover) 這個函式

```

**如果要使用，則方法如下：**

```
const introduction3 = introduction2.bind(novelABC)
```

**必須呼叫才會執行**

```
introduction3('It is cool!', 'yes')
// 印出 The novel ABC is about family. It is funny. It is cool! Cover: yes
```

# 預設綁定 (default binding)

預設綁定可能發生在當 JavaScript 遇到不符合隱式綁定 (implicit binding) 和顯式綁定 (explicit binding) 的情況時，便自動將 this 綁定到全域物件 (global object) 。例如：

```
function abc() {
  console.log(this)
}
abc() // 印出 Window 物件
```

# new 綁定 (new binding)

當我們使用建構函式 (constructor function) 產生新物件，就會需要使用關鍵字 new。
**過程：產生新物件 → this 被指定成剛新生成的空物件。**
例如：

```
function Introduction (name, about, impressions, cover) {
  this.name = name
  this.about = about
  this.impressions = impressions
  this.cover = cover
  console.log(`The novel ${this.name} is about ${this.about}. ${this.impressions} Cover: ${this.cover}`)
}

// 使用 new，這時可以得到一個新的物件，並且新物件中會放入參數內的值
const newintroduction = new Introduction('star', 'dream', 'It is so touching.', 'yes')

console.log(newintroduction)
// 印出 newintroduction  {name: 'star', about: 'dream', impressions: 'It is so touching.', cover: 'yes'}
```

# 小結

**this 綁定的優先順序：**
new 關鍵字綁定 (new binding) > 顯式綁定 (explicit binding) > 隱式綁定 (implicit binding) > 預設綁定 (default binding)

---

參考資料：

- [箭頭函式對 this 的影響](https://pjchender.dev/javascript/js-arrow-function/)

- AC 教材
