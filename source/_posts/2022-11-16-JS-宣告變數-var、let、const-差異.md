---
title: JS 宣告變數 var、let、const 差異
categories: JavaScript
tags:
  - JavaScript
description: 此篇從幾個不同面向來探討 var、let、const 的差異。
abbrlink: 475274378
date: 2022-11-16 16:39:15
---

## 科普

> 再開始之前先簡單科普幾個名詞，有助於理解後面篇幅內容。

### Scope

> 白話文：程式碼的作用域。（想看 MDN 定義的請往這邊 [scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope)）

編譯器或 JavaScript 引擎藉由 `識別字名稱`（identifier name）去查找變數，查找的規則是依循 RHS、LHS，而 `RHS、LHS` 是有範圍限制的（如果沒有範圍限制，全部的都變成全域不就 GG 了），所以透過定義出了三種 scope type（global scope、function scope、block scope）來區分 `RHS、LHS` 的使用範圍。

更多 Scope 運作原理可以參考下方文章：
- summer 大大（超爆詳細）：[你懂 JavaScript 嗎？#12 函式範疇與區塊範疇（Function vs Block Scope）](https://cythilya.github.io/2018/10/19/function-vs-block-scope/)

### Hoisting
> 白話文：將陳述式（ Statement ）提升（Hoisting），其特性使其在定義前，使用也可以運行。

下方圖片中左邊程式碼因為 `Hoisting 特性` 使其可以運行，其效果可以參考圖片中右邊程式碼，想像 `Hoisting` 就是將宣告的部分提升至最前面，使其在宣告前使用不會因為`尚未定義` 就噴 not defined。

![](https://imgur.com/jAJH5Yd.png)

### TDZ
> 白話文：在 let、const 情況下`宣告後`以及`尚未賦予值`前都無法取用，因為會進入 TDZ（暫時性死區中）。

下方圖片透過具象化方式來幫助理解`暫時性死區`，範例中宣告時會進入 TDZ，當賦予值時才會離開。

![](https://imgur.com/Em9khl0.png)

{% note danger %}
科普就到這邊，下方開始會透過範例來解析 var、let、const 三者差異。
{% endnote %}

下方圖片是三者比較表，在後續的篇文中可以搭配範例一起服用。
![](https://imgur.com/wSB9bbW.png)

---

## var

> var 作用域`僅限`於`函式`。

若在其他區塊（ex：for、while、if）宣告 var，會被當成`全域`使用。

```js
// function scope
var msg = "我是誰";
function message() {
	var msg = "我在哪裡";
	console.log(msg);  // 我在哪裡
}
message()
console.log(msg);    // 我是誰

// 可以重複宣告，var 特性導致當成全域變數重複宣告並覆蓋。
var msg2 = "我是誰";
if (true) {
	var msg2 = "我在哪裡";
	console.log(msg2);   // 我在哪裡
}

console.log(msg2);     // 我在哪裡
```

---
## let

> let 作用域在 `{}、()` 。

若在作用域外使用變數，會因為 TDZ 特性造成 `Reference Error`。

```js
// block scope
console.log(msg); // ReferenceError: not defined
{
    let msg = "我是誰";
}

// 無法重複宣告
let msg2 = "我是誰";
msg2 = "我在哪裡";
let msg2 = "我在幹嘛";  // SyntaxError: Identifier 'msg' has already been declared
```

---
## const

> const 作用域在 `{}、()` 。

若在作用域外使用變數，會因為 TDZ 特性造成 `Reference Error`，且宣告時`必須`賦予值，否則會噴 `SyntaxError` 錯誤。

```js
console.log(msg);
{
    const msg; // SyntaxError: Missing initializer in const declaration
	const msg1 = 'success'
}

// 無法重複宣告
const msg2 = "我是誰";
const msg2 = "我在哪裡";  // SyntaxError: Identifier 'msg' has already been declared

// 不可修改，除非是值是 reference（物件、陣列）
const msg3 = "我是誰";
msg3 = "我在哪裡"; // // TypeError: Assignment to constant variable
```

---
## conclusion

{% note danger %}
三者最大差異是 `作用域 Scope`。
{% endnote %}

{% note info %}
最後用兩個問題來複習這篇文章，大家可以試著回答看看。
{% endnote %}

<details>
  <summary>Q1：const 可不可以修改？</summary>
  <answer>A1：基礎型別不行，但物件型別可以，因為const 僅限制無法修改 value（無法重新賦予值），但可以修改 reference 原物件的值。</answer>
  <answer>補充：使用 <code>freeze()</code>，可以防止物件 reference 的 property 被更動。</answer>
</details>

<details>
  <summary>Q2：對於 JS 來說 const 效能勝過 let 和 var，那應該多使用 const 嗎？</summary>
  <answer>A2：const 特點是防止變數被改變，應該照這個邏輯去規劃程式中哪些是可以修改以及不能修改的，並讓程式碼透過 const 達到這個目的，以及提升可讀性，並非一昧盲目使用 const。</answer>
</details>