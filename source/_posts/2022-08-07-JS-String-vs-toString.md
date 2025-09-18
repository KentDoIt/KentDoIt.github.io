---
title: JS String vs toString
categories: JavaScript
tags:
  - JS 釘孤枝
  - JavaScript
description: 此篇主要探討 String()、toString() 兩種轉換字串方法的差異，以及使用情境。
abbrlink: 4119142473
date: 2022-08-03 12:17:59
---

## String()

{% note primary %}
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/String)：屬於建構函式，會將對象轉換為[字串](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)。
{% endnote %}

優點：所有型別都能轉換為字串。
缺點：`不支援`轉進位制轉換。

```js
String(null); // 'null'
String(undefined); // 'undefined'
```

## toString()

{% note primary %}
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/toString)：屬於方法，會回傳指定對象的字串樣子。
{% endnote %}

優點：支援進位制轉換。
缺點：無法轉換  `null` 、 `undefined`（會報 Error）。

```js 進位制轉換
// 括號中填寫進位對應數值

let num = 10;

// 二進位制
num.toString(2); // '1010'

// 八進位制
num.toString(8); // '12'

// 十進位制
num.toString(10); // '10'

// 十六進位制
num.toString(16); // 'a'
```

解法：可以使用 `try catch` 或 `短路求值` 來預防 Error。

```js
let toNum = null;

// 解法一
try {
  toNum.toString();
} catch (exception) {
  //  TypeError: Cannot read properties of null (reading 'toString')
  console.log(`${exception.name}: ${exception}`)
}

// 解法二
toNum && toNum.toString();
```

## different point

> null、undefined

toString：無法轉為字串（會噴 Error）
String：可以正確轉為字串

```js
let num = null;

console.log(num.toString()); // TypeError: Cannot read properties of null (reading 'toString')
console.log(String(num));    // 'null'
```

> 進位制

toString：可以正確轉換
String：無法正確轉換

```js
let num = 10;
console.log(num.toString(2)); // '1010'
console.log(String(num));     // '10'
```

## conclusion

{% note default %}
需要做進位制轉換就使用 `toString`，否則就使用 `String`。
使用 `String` 時預防噴 Error。
{% endnote %}
