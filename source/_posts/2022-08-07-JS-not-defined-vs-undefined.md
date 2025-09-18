---
title: JS not defined vs undefined
categories: JavaScript
tags:
  - JS 釘孤枝
  - JavaScript
description: 探討 undefined、not defined 兩者區別以及注意事項。
abbrlink: 4170310433
date: 2022-07-30 10:25:08
---

![](https://imgur.com/3uvmsGO.png)

## undefined

{% note primary %}
[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/undefined)：屬於 `primitive value` 之一、全域物件的 `property`。
{% endnote %}

> Tips

1. 預設值
表示變數 `尚未宣告` 或者是已經宣告了但 `尚未賦值`。

2. 特殊關鍵字
可以當作一個值去賦予不會噴錯，但 `千萬不要` 這麼做。

```js
let b;
console.log(b); // undefined

b = undefined; // 不要這麼做
```

{% note default %}
下方會演示兩種 undefined 錯誤應用，並解析問題。
{% endnote %}

### 不建議賦值為 undefined

{% note danger %}
問題：造成無法看出此變數是否有被賦予值。
{% endnote %}

解法：

1. 使用 null，更符合空值的意義。
2. 宣告變數不帶值，預設就會賦予 undefined。

```js
// 不建議寫法
let a = undefined; // undefined

// 替代方式
let a;             // undefined
let a = null;      // undefined
```

### 不建議變數名稱命名為 undefined

{% note danger %}
問題：javascript 會過，但無法正常運作。
{% endnote %}

解法：

1. 直接不要用 undefined 命名
2. 不要用 var 改用 let、const（ES6 中已修正此問題，因此無法使用 undefined 命名）

```js
// 不建議寫法
var undefined = 'un';
console.log(undefined); // undefined，無法正常顯示值

// let、const
let undefined = 'un';   // Uncaught SyntaxError: Identifier 'undefined' has already been declared
const undefined = 'un'; // Uncaught SyntaxError: Identifier 'undefined' has already been declared
```

## not defined

{% note primary %}
屬於 error，會噴錯誤訊息。（throw a ReferenceError exception）
{% endnote %}

> Tip

- 常見的原因是使用了`尚未宣告`的變數或函式，所導致的 Error。

```js
console.log(c); // ReferenceError: c is not defined
```

---

## different

- undefined：宣告(declaration) 後`尚未賦予值`。
- not defined：`尚未被宣告`(declaration)。

```js
let b;
console.log(b); // undefined
console.log(c); // ReferenceError: c is not defined
```

## conclusion

![](https://imgur.com/bGXwBgw.png)

## reference

- [JavaScript 核心觀念(7)-執行環境與作用域-not defined VS undefined](https://hsiangfeng.github.io/javascript/20200510/2581786882/)
- [Day04　undefined與not defined](https://ithelp.ithome.com.tw/articles/10190904)

延伸閱讀

- [JS null vs undefined](https://kentdoit.github.io/javascript/3007200680/)
