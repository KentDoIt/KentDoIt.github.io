---
title: JS 樣板字面值 Template Literal、標籤樣板字面值 Tagged Template Literals
categories: JavaScript
tags:
  - ES6
  - JavaScript
description: 此篇會介紹 ES6 新增的「板字面值 Template Literal」。
abbrlink: 2204307334
date: 2022-06-23 12:01:24
---

## 樣板字面值 Template Literal

{% note primary %}
[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Template_literals)：用來進行多行字串、字串串接以及字串插補（string interpolation），在`先前`的版本 ES2015 中被稱為「樣板字串（template strings）」。
{% endnote %}

語法：

{% codeblock lang:shell line_number:false %}
`string text ${expression} string text`
{% endcodeblock %}

{% note default %}
兩個反引號` `` `並加上 `${expression}` 來插入變數、表達式。
{% endnote %}

```js e.g. 樣板自面值
let s = '我';
console.log(`${s}是誰，${s}在哪裡`);
```

> Tips

- 反引號的內容會保留所有的 `換行` 和 `空白`。
- 不能插入陳述式，否則會噴 Error。

```js
console.log(`我是誰  ，\n  我在哪裡`);
console.log(`這是陳述式範例：變數宣告${ let str}`); // SyntaxError: Missing } in template expression
```

{% note default %}
Q：那為什麼會需要樣板字面值？其它方式不行嗎？
A：可以，但樣板字面值簡化`串接過程`，使串接變得更`容易簡潔`。
{% endnote %}

{% note default %}
下面會透過幾個範例，來演示舊字串串接、樣板字面值兩者在不同情境的差別。
{% endnote %}

### 語法差異

{% note info %}
舊字串串接：會使用`單引號`或`雙引號` 來`包裹字串`，再透過 `+` 的方式來串接。
樣板字面值：兩個反引號 `` 並加上 `${expression}` 來插入變數、表達式。
{% endnote %}

```js e.g. 新舊寫法差異
let str1 = '世界';
let str2 = '和平';

// 舊字串串接
console.log(str1 + str2 + ' & 愛');

// 樣板字面值
console.log(`${str1}${str2} & 愛`);
```

### 多行串接

```js e.g. 多行串接
let money = 10;

// 舊字串串接（需要使用到反斜線來換行）
let template = '<ul>\
  <li>' + money + '元 </li>\
  <li>' + money + '元 </li>\
</ul>';

// 樣板字面值（會保留所有換行和空白）
let template = `
  <ul>
    <li> ${money}元 </li>
    <li> ${money}元 </li>
  </ul>
`;
```

### 跳脫字元

{% note default %}
Q：那如果需要將反引號作為字串怎麼辦？
A：可以使用跳脫字元。
{% endnote %}

```js e.g. 跳脫字元
// 在字元前面加上反斜線（backslash）
console.log(`我厶\`誰`); // 我厶`誰
```

### 複雜用法

{% note default %}
上面範例中已充分暸解如何使用樣板字面值，來做字串串接和字串插補，下方在用一個範例來演示，結合函式、判斷式、巢狀的應用。
{% endnote %}

```js e.g. 函式 + 判斷式 + 巢狀
let moneys = [10, 0];
let template = `<ul>
  ${moneys.map((money) => {
    if (money) {
        return `<li> ${money}元 </li>`;
      } else {
        return `<li> 沒錢 </li>`;
      }
    }).join('')
  }
</ul>`;
```

{% note default %}
不敢想像用傳統串接寫的話會變成什麼樣子 QQ
{% endnote %}

---

## 標籤樣板字面值 Tagged Template Literals

{% note primary %}
[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Template_literals#%E6%A8%99%E7%B1%A4%E6%A8%A3%E6%9D%BF%E5%AD%97%E9%9D%A2%E5%80%BC)：屬於樣板字面值的進階用法，常用於函式參數傳入。
{% endnote %}

語法：

- 函式後方將括號替代為樣板字面值

```js e.g. 簡化了一般函式呼叫的括號
// 一般函式參數
console.log('標籤樣板字面值');

// 標籤樣板字面值
console.log`標籤樣板字面值`;
```

{% note default %}
如果樣板字面值中沒有使用到 `${}`，那和一般函式呼叫帶入的參數沒什麼差異，那看起來也沒多厲害？不不不精彩的正要開始。
{% endnote %}

### 樣板字面值傳參數特性

- 將樣板字面值內容中含有 `${}` 切為 `多個參數`，將其組成一個陣列傳入函式中。

參數格式

- 第一個參數（陣列）：除了 `${}` 之外的內容。
  - 從 `${}` 位置切割，並會以字串型態存入陣列中。
  - 要注意就算 `${}` 後面沒有其他字串，但還是會有一個空字串存入陣列中
- 第二個參數：第一個 `${}`
- 第三個參數：第二個 `${}`（...依此類推下去）

![](https://imgur.com/N24S0H1.jpg)

{% note default %}
${} 後面沒有其他字串，但還是會有一個空字串存入陣列中。
例如：一個 ${} 會切兩個字串，兩個 ${} 會切三個字串。
{% endnote %}

```js e.g. 標籤樣板字面值
let str1 = '標籤';
let str2 = '標籤樣板字面值';
function fn(str, arg) {
  console.log(str);  // str=['演示', '', '範例']
  console.log(arg);  // arg='標籤'
}

fn`演示${str1}${str2}範例`;
```

{% note default %}
變數的部分，也可以使用`其餘運算子`來一次接收全部變數。
{% endnote %}

```js e.g. 其餘運算子接收所有變數
let str1 = '標籤';
let str2 = '標籤樣板字面值';
function fn(str, ...arg) {
  console.log(str);  // str=['演示', '', '範例']
  console.log(arg);  // arg=['標籤', '標籤樣板字面值']
}

fn`演示${str1}${str2}範例`;
```

{% note default %}
通常 `${}` 數量是不固定的，因此可以使用其餘運算子將 `${}` 的參數組成陣列來使用。
{% endnote %}

```js e.g. 標籤樣板字面值 + 其餘運算子
let str1 = '標籤樣板字面值';
let str2 = '其餘運算子';

function fn(str, ...args) {
  console.log(str, args); 
  // str=['演示', '+', '範例'], arg=['標籤樣板字面值', '其餘運算子']
}

fn`演示${str1}+${str2}範例`;
```

{% note danger %}
要注意第一個參數永遠只會傳入非 `${}` 的字串，所以請不要在第一個參數使用其餘參數，因為沒有必要。
{% endnote %}

{% note default %}
標籤樣板字面值也常用來預防 XSS 攻擊，請看下方範例。
{% endnote %}

- 透過將 `${}` 內容中含有`<`、`>` 與 `&` 的符號替換成 ASCII Code，也就是單純的字串。

```js e.g. 預防 XSS 攻擊 https://israynotarray.com/javascript/20211114/136128318/ src
function convertHTML (strings, ...keys) {
  return strings.map((str, i) => `${str}${keys[i] ? keys[i].replace(/&/g, '&amp;').replace(/</g, '&lt;').replace(/>/g, '&gt;'): ''}`).join('');
}

convertHTML`<p>超連結：${Url}</p>`;
```

## reference

- [ES6 章節：Template Literial](https://medium.com/vicky-notes/es6-%E7%AB%A0%E7%AF%80-template-literial-7309b9e3bebb)
- PJ 大 [[筆記] JavaScript ES6 中的模版字符串（template literals）和標籤模版（tagged template）](https://pjchender.blogspot.com/2017/01/javascript-es6-template-literalstagged.html)
- Ray [JavaScript 核心觀念(72) - ES6 章節：Template Literial - 標籤樣板字面值](https://israynotarray.com/javascript/20211114/136128318/)
