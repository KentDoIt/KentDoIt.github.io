---
title: JS null vs undefined
categories: JavaScript
tags:
  - JS 釘孤枝
  - JavaScript
description: 此篇探討在 JS 中 null、undefined 的差異。
abbrlink: 3007200680
date: 2022-07-25 10:24:54
---

## 定義

null：表示變數 `沒有值`（空值）。
undefined：預設值，表示變數 `尚未宣告` 或者是已經宣告了但 `尚未賦值`。

```js
let def;         // undefined
let text = null; // null
```

> 常見應用

[undefined](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/undefined)

- 常用於表示尚未賦予值（預設值），或是錯誤的沒有值（ex：物件沒有的屬性、呼叫函數時缺少的參數）。

```js
let obj = { 
  name: 'kent',
}
console.log(obj.age); //undefined
```

[null](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/null)

- 作為初始化（init）表示 `現在` 沒有值之後 `可能會` 有值。

```js
let num = 0;
num = 100;

// init
let num = null;
```

{% note default %}
發覺和其他語言不同在 JS 中，大多是人不會將值設定為 `null`，而是依照不同型別來設置 init，我猜測和 JS 擁有非常多可惡的自動轉型規則有關係。
{% endnote %}

```js
let obj = {};
let num = 0;
let str = '';
```

{% note default %}
JS 中某些特定情況時會回傳 null。（下方介紹兩個在 JS 中會出現 null 的情境）
{% endnote %}

情境一：閉包 closure、作用域鏈 scope chain

- 當不存在的變數、函式，JS 會往上一層去尋找，直到 null 就會噴 not define error。。

情境二：獲取 DOM 元素時

- 若元素不存在會回傳 null。

## same point

1. 基礎型別（primitives type）
2. null、undefined 都是假值 Falsy (Boolean 為 false)

```js
console.log(Boolean(undefined));  // false
console.log(Boolean(null));       // false

console.log(undefined == null);   // true，因為都是 false
```

{% note default %}
想暸解 undefined == null 為什麼是 true，可以參考這篇[文章](https://kentdoit.github.io/javascript/2972146539/#x3D-x3D)（裡面有提到是因為 == 的規則導致）
{% endnote %}

---

## different point

{% note default %}
下方透過四個方向來演示兩者結果差異。
{% endnote %}

> Number

null：`Number(null)` 結果為 `0`
undefined：`Number(undefined)` 結果為 `NaN`。

{% note default %}
NaN：代表不是數值（not a number）
{% endnote %}

> typeof

null：`typeof undefined` 結果為 `undefined`。
undefined：`typeof null` 結果為 `Object`。

> JSON

null：`有效`的 JSON 值。
undefined：`無效`的 JSON 值。

> keyword

null：是 keywords。（沒辦法作為變數名稱）
undefined：不是 keywords。（但 `千萬不要` 將變數名稱設為 undefined）

## conclusion

{% note default %}
JS 中變數 undefined 比起 null，更相似其它語言的 NULL。（因為預設值）
{% endnote %}

{% note danger %}
除非很熟悉，不然新手一律不建議將變數設置 `null` 或 `undefined`。
{% endnote %}

<details>
  <summary>有興趣可以參考下方幾篇 stackoverflow 的討論</summary>
  <a target="_blank" rel="noopener" href="https://stackoverflow.com/questions/16474796/set-javascript-variable-null-or-leave-undefined/16474917#16474917">1. Set JavaScript variable = null, or leave undefined?</a></br>
  <a target="_blank" rel="noopener" href="https://stackoverflow.com/questions/13142912/when-do-i-initialize-variables-in-javascript-with-null-or-not-at-all/13142982#13142982">2. When do I initialize variables in JavaScript with null or not at all?</a></br>
  <a target="_blank" rel="noopener" href="https://stackoverflow.com/questions/6604749/what-reason-is-there-to-use-null-instead-of-undefined-in-javascript/48197438#48197438">3. What reason is there to use null instead of undefined in JavaScript?</a></br>
  <a target="_blank" rel="noopener" href="https://stackoverflow.com/questions/6604749/what-reason-is-there-to-use-null-instead-of-undefined-in-javascript/6604783#6604783">4. What reason is there to use null instead of undefined in JavaScript?</a></br>
</details>

## reference

- 卡斯伯老師：[鐵人賽：動態型別的 JavaScript](https://wcc723.github.io/javascript/2017/12/08/javascript-typeof/)