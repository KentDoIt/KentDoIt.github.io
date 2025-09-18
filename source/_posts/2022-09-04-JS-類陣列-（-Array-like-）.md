---
title: JS 類陣列 （ Array-like ）
categories: JavaScript
tags:
  - JavaScript
description: 此篇主要講解 Array-like 和一般陣列有什麼差異，最後也會介紹三種將類陣列轉成陣列的方法。
abbrlink: 3574120313
date: 2022-08-20 11:54:00
---

## Array-like

{% note primary %}
Array-like object 就是一個類陣列物件（很像陣列的物件），和一般陣會最大差異就是，類陣列`沒有`陣列內建的方法。
{% endnote %}

### 常見的類陣列

- [arguments object](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments)
    - 函式中用來接收所有引數。
- [HTMLCollection](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection)
    - 透過方法（ex：getElementsByTagName）取得的 DOM 物件。
- [NodeList](https://developer.mozilla.org/en-US/docs/Web/API/NodeList)
    - 透過方法（ex：querySelectorAll）取得的節點集合。

{% note danger %}
要注意，類陣列 `不是真正` 的陣列。
{% endnote %}

### 類陣列特性

- 可以使用`索引值`取得值。
- 可以使用 `length` 屬性。
- 可以使用 `callee` 屬性。
	- callee 可以當作是一個指標，指向函式本身。
	- 對於匿名函式中特別有用，例如：讓匿名函式也可以進行遞迴（但在 ES5 嚴格模式底下已禁用，詳細原因請參考 [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/arguments/callee)）
- `沒有` 陣列內建的 `方法`。（ex：indexOf、concat、forEach）

```js
function add(num1, num2) {
	let arr = [2000, 22];
	console.log(arguments[0]);     // 2000
	console.log(arguments.length); // 2
	console.log(arguments.callee); // function code
	console.log(arguments, arr);
};

let constNum = add(2000, 22);
```

下方圖片為 `console.log(arguments, arr);` 的結果，可以從 Prototype 看出 arguments object 中`沒有`內建陣列方法。

![](https://imgur.com/tsnSGYm.jpg)

{% note default %}
雖然上述類陣列看起來這麼不方便，但可以將類陣列`轉為`陣列，來使用陣列方法。
{% endnote %}

---

## 類陣列轉陣列的三種方法

{% note default %}
下列介紹的三種都屬於淺拷貝（shallow-copied）的方式
{% endnote %}

- ES5 [slice](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice#array-like_objects)：`Array.prototype.slice.call(arguments);`
- ES6 [Array.from](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/from)：`Array.from(arguments)`
- ES6 [Rest parameters（解構式）](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/rest_parameters)：`[...arguments];`

```js
function add(num1, num2) {
	let arr1 = Array.prototype.slice.call(arguments);
	let arr2 = [...arguments]; // 寫法相對簡潔
	let arr3 = Array.from(arguments);
	console.log(arr1, arr2, arr3);
}
let constNum = add(2000, 22);
```

由下方圖片的中的 Prototype 看得出來三者皆是陣列。

![](https://imgur.com/2HqYyra.jpg)

{% note default %}
下方會透過效能、支援度這兩個方面來對這三種方法做比較
{% endnote %}

---

## performance 效能差異

> 圖片來自 [JS array from an array-like object](https://catalin.red/js-array-from-array-like-object/) 文章，內文中有對上述提到的三種方法，進行效能的評測。

![](https://imgur.com/WlvvvyC.jpg)

{% note default %}
從上方圖片可得知 `slice` 方法的速度是較快的，優於另外兩種方法不少。
{% endnote %}

### support 支援度

> 下方截圖是來自 [caniuse](https://caniuse.com/)，比對上述三種方法對於瀏覽器的支援度。

![](https://imgur.com/wANgTbr.jpg)
![](https://imgur.com/sGiyYxv.jpg)
![](https://imgur.com/R7GWrno.jpg)

{% note default %}
最後結果可以得知 `slice` 方法的支援度是最好的。
{% endnote %}

## conclusion

{% note default %}
上述三種方法若從效能和支援度方面來看，`slice` 勝。
{% endnote %}

## reference

- [[12] 值 - 陣列、類陣列](https://zh-tw.coderbridge.com/series/9e5162da940f473a9f1cfeece124ee98/posts/3df8bdb9dfdd4fef9f8b2d1b12eb5bf0)
- [JS array from an array-like object](https://catalin.red/js-array-from-array-like-object/)
- [JavaScript - Difference between Array and Array-like object](https://stackoverflow.com/questions/29707568/javascript-difference-between-array-and-array-like-object)
