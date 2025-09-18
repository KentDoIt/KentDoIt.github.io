---
title: JS 空值合併運算子 Nullish coalescing operator
categories: JavaScript
tags:
  - 運算子
  - JavaScript
abbrlink: 3525467973
date: 2022-06-20 13:48:52
description: 此篇會介紹 JS 中常用技巧「空值合併運算子」，以及其優缺點。
---

{% note default %}
建議先閱讀前一篇「[JS 短路求值 Short-circuit evaluation](https://kentdoit.github.io/javascript/350805089/)」後再接下去閱讀。
{% endnote %}

{% note primary %}
[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)：`??` 左邊是 `null` 或 `undefined` 則不會執行右邊。
{% endnote %}

[MDN 語法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator#syntax)

{% codeblock lang:shell line_number:false %}
leftExpr ?? rightExpr
{% endcodeblock %}

e.g. 判斷是否使用過

{% note default %}
情境：假設有一間銀行，發給新辦用戶 100 摳
{% endnote %}

```js
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

person1.wallet ?? {personOld, ...{wallet: 100}}; // {person1: {…}, wallet: 0}
person2.wallet ?? {personNew, ...{wallet: 100}}; // {person2: {…}, wallet: 100}
```

e.g. 預設值

- 再賦予值的過程透過 `??` 來設置預設值，只要 `?? 左邊` 為 null、undefined 則就會賦予 `?? 右邊` 為新值。

```js
let n;
let num = n ?? '0';
```

> 使用 tips

{% note danger %}
不能與 &&、|| 一起使用。（不然會噴 SyntaxError）
{% endnote %}

```js
null || undefined ?? "foo"; // 噴 SyntaxError
(null || undefined ) ?? "foo"; // 返回 "foo"
```

## conclusion

- `空值合併 ??` 相較於 [`短路求值 ||`](https://kentdoit.github.io/javascript/350805089/) 更加嚴謹。

![](https://imgur.com/2sYcGjJ.jpg)

## reference

- [谈谈 JavaScript 中的空值合并操作符 Nullish coalescing operator](https://cloud.tencent.com/developer/article/1954297)
