---
title: 【觀念 09】介紹閉包（closure）
date: 2022-09-21 13:41:43
tags:
  - [JavaScript]
categories:
  - [基礎觀念]
---

函式在建立時會產生閉包，捕捉外部函式的變數及環境，可以使變數繼續存活於函式內部。

<!-- more -->

# 閉包例子：

```
let outer = function (parOuter){
  const num = 1
  let inner = function (parInner){
    const addInner = num + parInner
    const addOuter = num + parOuter

    console.log('Outer', parOuter) // 2
    console.log('Inner', parInner) // 5
    console.log('addInner', addInner)  // 6
    console.log('addOuter', addOuter) // 3
  }

  console.dir('fun', inner)
  return inner
}

const func = outer(2)
func(5)

console.dir(func)
// 可以看到 function inner(parInner)
// 存了 Closure (outer) {parOuter: 2, num: 1}

// 當執行 func(5)，上面程式碼的 function inner 可以視為如下：
  (function inner(parInner) {
	const addInner = num + parInner
	const addOuter = num + parOuter
	...
  })(5)

```

# 閉包的用途：

規定好資料的操作方式，避免遭到直接修改。

例如：

```
const customer = {
  name: 'big apple',
  appleCount: 0,
  calCount() {
    this.appleCount += 1
  }
}

// 希望操作方式
customer.calCount() // +1

console.log('customer', customer.name) //customer big apple
console.log('customer', customer.appleCount) // customer 1

// 不希望操作方式
customer.appleCount = 100

console.log('customer', customer.appleCount) // customer 100

```

# 運用閉包 1：

```
const customer = () => {
  let name = 'big apple'
  let appleCount = 0
  return {
    name,
    getCount() {
      console.log(appleCount)
    },
    calCount() {
      // 閉包記住了 appleCount
      appleCount += 1
    }
  }
}

// build customer1
const customer1 = customer()

customer1.calCount() // 執行 +1
customer1.calCount() // 執行 +1
console.log(customer1.getCount()) // 得到 2

```

上面可以避免 customer1.appleCount = X 的方式直接修改值。

# 運用閉包 2：

```
function Apple() {
  this.name = 'big apple'
  this.count = 0
  let appleCount = this.count

  let calCount = function (num) {
    // 閉包記住了 appleCount
    return appleCount += num
  }
  this.increment = function () { calCount(1) }
  this.decrement = function () { calCount(-1) }
  this.countVal = function () { return appleCount }

  return {
    // 開放下面的值可以被取用
    name: this.name,
    increment: this.increment,
    decrement: this.decrement,
    countVal: this.countVal,
  }
}

const customer1 = new Apple()
console.log('customer1', customer1.name) // big apple
console.log('customer1', customer1.count)
// undefined 可以避免 customer1.count 直接被取值和修改
customer1.increment() //1
console.log('customer1 >> +1', customer1.countVal())

customer1.increment() //2
console.log('customer1 >> +1', customer1.countVal())

customer1.decrement() //1
console.log('customer1 >> -1', customer1.countVal())

const customer2 = new Apple()

customer2.increment() //1
console.log('customer2 >> +1', customer2.countVal())

customer2.increment() //2
console.log('customer2 >> +1', customer2.countVal())

```

customer1 和 customer2 實例，得到的值互不影響。
如果希望使用程式的人按照你寫的規則，就會需要閉包的幫忙。

---

參考資料：

- [Closure 閉包](https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/closure.html)
- [為什麼我們需要閉包(Closure)？](https://nissentech.org/why-do-we-need-closure/)
- [JS Closure 閉包](https://ithelp.ithome.com.tw/articles/10212048)
