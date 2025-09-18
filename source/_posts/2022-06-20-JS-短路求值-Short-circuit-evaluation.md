---
title: JS 短路求值 Short-circuit evaluation
categories: JavaScript
tags:
  - 運算子
  - JavaScript
abbrlink: 350805089
date: 2022-06-20 13:34:38
description: 此篇會介紹 JS 中常用技巧，將 &&、|| 運用「短路求值」的方式來提升 coding 效率。
---

{% note primary %}
快速複習一下[邏輯運算子](https://kentdoit.github.io/javascript/1047366401/)：「當 AND 的第一個運算數的值為 false 時，其結果必定為 false；當 OR 的第一個運算數為 true 時，最後結果必定為 true。」
{% endnote %}

{% note default %}
不熟悉 `真值（truthy）`、`假值（falsy）` 可以參考這篇[文章](https://kentdoit.github.io/javascript/1047366401/#%E7%9C%9F%E5%80%BC%EF%BC%88truthy%EF%BC%89%E3%80%81%E5%81%87%E5%80%BC%EF%BC%88falsy%EF%BC%89)。
{% endnote %}

> 口訣

- `&&` 左邊是 `假值(falsy)` 則不會執行右邊。
- `||` 左邊是 `真值(truthy)` 則不會執行右邊。

{% note default %}
下方範例運用 &&、|| 特性，演示將左右兩邊的 `表達式` 達到和 if 判斷式一樣的效果。
{% endnote %}

e.g. 使用 `||` 來實作賦予預設值功能

```js
let a = null;

// 短路求值寫法
let b = a || 1; // a 不是真值所以會執行右邊，將 1 賦予到 b 變數上。

// if 寫法
if (!a) {
  let b = a;
} else {
  let b = 1;
}

// 三元運算寫法
let b = a ? a : 1;

```

e.g. `&&` 透過判斷 `真假值` 來決定是否執行表達式（也可以執行函式）

```js
let person1 = {
  name: 'kent',
  age: 18
}

// 短路求值寫法（僅適用於只有一種結果的判斷式）
person1.age >= 18 && console.log(`${person1.name} 已成年`); // person1.age >= 18 是 true 所有會執行右邊 console.log

// if 寫法
if (person1.age >= 18) console.log(`${person1.name} 已成年`);
```

e.g. 判斷物件屬性是否存在

```js
let school = {
  info: {
    student: {
      name: 'kent',
      age: 18,
    },
  },
};

// 短路求值寫法
let name = obj.info.teacher && obj.info.teacher.name; // 會透過先判斷上一層屬性是否存在，來預防噴錯

// if 寫法
let name = obj.info.teacher.name; // TypeError: Cannot read properties of undefined (reading 'name')

if (obj.info.teacher) {
  let name = obj.info.teacher.name;
}
```

> 短路求值 tips

1. `短路求值` 使用表達式的情況下，只適用於沒有 else 的判斷式，需要有 else 可以使用 `三元運算子`。
2. `短路求值` 不能帶 return。

```js
function isAdult(age) { 
  // 判斷是否為假值

  // 短路求值寫法（短路求值帶 return 會噴 error）
  age && return '資料錯誤'; // SyntaxError: Unexpected token 'return'

  // if 寫法
  if (!age) {
    return '資料錯誤';
  }


  // 下方範例不適合用短路求值，因為會缺少 else 表達式。
  let res
  // 短路求值寫法
  res = (age >= 18) && '已成年';

  // 三元運算子寫法
  res = age >= 18 ? '已成年' : '未成年';

  return res;
}

isAdult(null);  // 資料錯誤
isAdult(18); // 已成年
isAdult(17);  // 未成年
```

### 詬病「不夠嚴謹」

{% note default %}
👨‍💻 當變數為 `false`、`0` 或空字串 `""` 其中之一時， 則布林值也會轉型為`假值 (falsy)`，對於判斷變數是否使用過來說是相當不方便的一件事。
{% endnote %}

```js e.g. 判斷某個變數是否使用過，有的話就作初始化。
let time = 1 - 1;

// 判斷變數是否使用過，有的話將其初始化
if (time) {
  // 初始化失敗
  time = null;
}

// 初始化失敗
time = time && null;
```

```js e.g. 假設有一間銀行，發給新辦用戶 100 摳，結果連舊客戶都發了...
let person1 = {
  name: 'kent',
  age: 18,
  wallet: 0
};

let person2 = {
  name: 'clark',
  age: 20,
  wallet: null
};

person1.wallet || {person1, ...{wallet: 100}}; // {person1: {…}, wallet: 100}
person2.wallet || {person2, ...{wallet: 100}}; // {person2: {…}, wallet: 100}

// 發現活動還沒開跑，已經發了幾百萬出去
```

![部署後的工程師](https://imgur.com/yNnVS12.jpg)

{% note default %}
javascript 後來增加了一種 [`空值合併運算子`](https://kentdoit.github.io/javascript/3525467973/) 用來解決不夠嚴謹的問題。
{% endnote %}

## conclusion

![||、&& 使用口訣](https://imgur.com/w9OtDHw.png)

## reference

- [用邏輯判斷 ||(OR) 及 &&(AND) 來改寫 if (短路求值 Short-circuit evaluation)](https://bolaslien.github.io/blog/2021/06/18/js-short-circuit-evaluation/)
