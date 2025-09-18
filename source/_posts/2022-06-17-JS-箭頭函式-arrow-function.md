---
title: JS 箭頭函式 arrow function
categories: JavaScript
tags:
  - 箭頭函式
  - this
  - arguments
  - JavaScript
description: 此篇會用範例來了解什麼是箭頭函式以及特性。
abbrlink: 2638792469
date: 2022-06-17 11:02:37
---

{% note default %}
箭頭函式是 ES6 新的語法糖，白話文來說，箭頭函式是傳統函式的`縮寫`，但有一些特性和邏輯不盡相同尤其是 this 的部分。
{% endnote %}

[MDN 語法](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

```js
([param] [, param]) => {
  statements
}

param => expression
```

## 縮寫特性

> 用一張圖來看箭頭函式這個語法糖究竟簡化了些什麼？
{% note default %}
將 function 簡化為`箭頭 =>`。
{% endnote %}

![](https://imgur.com/okxB6Sa.jpg)

### 單行型式縮寫

> 規則

1. 大括號括號可省略。
2. 參數只有一個時，可以把參數的小括號省略。（沒有參數、兩個參數以上時不可省略小括號）
3. 若需要回傳值則可省略 return。

![](https://imgur.com/hdFhyyI.jpg)

### 縮寫 tips

{% note default %}
下方會用範例來演示撰寫箭頭函式時需要注意的 tips。
{% endnote %}

- 只有一個參數才不用加 `括號()`，否則會噴錯。

```js
// 錯誤
const arrowFn = => '箭頭函式'; // SyntaxError: Unexpected token '=>'
const arrowFn = item, index => '箭頭函式'; // SyntaxError: Unexpected token '=>'

// 正確
const arrowFn = () => '箭頭函式';
```

- 參數、return 值有包含 `大括號 {}` 物件，則需要在外圍加上`小括號()` 將其包裹起來。

```js
// 錯誤
const arrowFn = {name, sex} => name;                 // SyntaxError: Malformed arrow function parameter list
const arrowFn2 = () => {name: 'kent', sex: 'male'};  // SyntaxError: Unexpected token ':'

// 正確
const arrowFn = ({name, sex}) => name;
const arrowFn2 = () => ({name: 'kent', sex: 'male'});
```

## this

{% note primary %}
箭頭函數中的 this 在`定義`時就綁定了不會更改，會將其綁定在定義的`執行環境`，另一種說法是會指向上一層的 this。
{% endnote %}

```js e.g. this 指向
// 傳統函式
const obj = {
  fn: function() {
    return this;
  },
};

// 箭頭函式
const obj1 = {
  arrowFn: () => this
};

console.log(obj.fn());         // obj 物件
console.log(obj1.arrowFn());   // window 物件
```

{% note danger %}
但有幾種特例情況會造成箭頭函式的 this 指向改變，而傳統函式的 this 物件指向是可變的，會隨著其呼叫的物件而有所改變。
{% endnote %}

{% note default %}
下方範例會掩飾在不同的情況下，箭頭函式以及傳統函式兩者 this 指向的結果。（不會對 this 做詳細的介紹）
{% endnote %}

### 嚴格模式 simple call

- 傳統函式：指向 window，當函式 `沒有` 指定`呼叫物件`時，默認就會綁定在 `全域物件 window`。
- 箭頭函式：指向 undefined，因為不允許默認綁定。

```js
const fn = () => { 
  'use strict';
  return this; 
};
console.log(fn()); //window物件

const arrowFn = function() {
  'use strict'; 
  const arrowFn2 = () => this;
  console.log(arrowFn2()); //undefined 
  return this; 
}
console.log(arrowFn()); //undefined 
```

### 物件實字（object literal）方法

- 傳統函式：指向 obj 物件，因此印出傳統函式。
- 箭頭函式：指向上一層 window 物件，由於物件特性沒有定義 type 會回傳 undefined。

```js
// 傳統函式
const obj = {
  type: '傳統函式',
  fn: function() {
    console.log(this.type);  // 傳統函式
  },
};

// 箭頭函式
const obj1 = {
  type: '箭頭函式',
  arrowFn: () => {
    console.log(this.type);  // undefined
  },
};

obj.fn();         
obj1.arrowFn();
```

![](https://imgur.com/wzpfSYj.jpg)

### call、apply、bind 方法

{% note default %}
這三種方法會透過傳入物件，並將函式 this 指向其物件。
{% endnote %}

- 傳統函式：指向 obj 物件，因此印出傳統函式。
- 箭頭函式：指向 window 物件，由於箭頭函式特性（this 在 `定義` 時就綁定了不會更改）使其無法重新改變 this 指向。

```js
let obj = {
  name: 'Kent'
}
const fn = function () {
  console.log(this);  // obj 物件
}
const fnArrow = () => {
  console.log(this);  // window 物件
}
fnArrow.call(obj); 
fn.call(obj);
```

### Prototype 物件原型

- 傳統函式：回傳 5，指向 Member 物件。
- 箭頭函式：回傳 undefined，指向上一層 window 物件。

```js
const type = "window";
const Member = function(name, age) {
  this.name = name;
  this.age = age;
};

Member.prototype.getAge = function() {
  console.log(this.age ,this); // 5, Member 物件
};
Member.prototype.getAgeArrow = () => console.log(this.age ,this); // undefined, window 物件

const customer = new Member('Kent', 5);
customer.getAge();
customer.getAgeArrow();
```

### DOM 監聽函式

{% note default %}
DOM 擁有自己的 this，當事件觸發時會將 this 指向觸發事件的元素本身。
{% endnote %}

- 傳統函式：函式被執行時指向元素本身。
- 箭頭函式：函式被執行時指向 window 物件。

```js
const btn = document.querySelector('button');
const obj = {
  type: '傳統函式',
  fn: function () {
    console.log(this, this.type); // DOM element, 'button'
  }
}
btn.addEventListener('click', obj.fn);


const obj1 = {
  type: '箭頭函式',
  fnArrow: () => {
    console.log(this, this.type); // // window 物件, undefined
  }
}
btn.addEventListener('click', obj1.fnArrow);
```

## arguments 參數

{% note danger %}
箭頭函式沒有 arguments 參數使用的話會噴 Error。
{% endnote %}

{% note default %}
解法：使用其餘參數（rest parameter）來替代，（arguments 是類陣列，其餘參數是陣列）
{% endnote %}

```js
const fn = function (parm) {
  console.log(arguments);
};
const arrowFn = (parm) => {
  console.log(arguments);
};
fn(1, 2, 3);      // Arguments(3)
arrowFn(1, 2, 3); // ReferenceError: arguments is not defined

// 其餘參數（rest parameter）
const arrowRestFn = (...parm) => {
  console.log(parm);
};
arrowRestFn(1, 2, 3); // [1, 2, 3]

```

## constructor、prototype

- 沒有原型（prototype）屬性，使用會回傳 undefined。
- 若使用於建構式（constructor），會在使用 new 時候噴 error。

{% note default %}
在實體化物件的時候會改變 this 的指向，指向新的實體物件，但由於箭頭函式無法指向新的實體物件所以會噴錯。
{% endnote %}

```js
const fn = function () {
  console.log(this);
};
const arrowFn = () => {
  console.log(this);
};
console.log(fn.prototype);      // {constructor: ƒ}
console.log(arrowFn.prototype); // undefined

new fn();
new arrowFn(); // TypeError: arrowFn is not a constructor
```

## Array.prototype、陣列方法

![正常傳統函式中的 Array 屬性](https://imgur.com/oQML1lh.jpg)

{% note default %}
箭頭函式由於沒有 [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) 這個屬性，因此無法使用陣列方法
{% endnote %}

```js
const calculate = {
  array: [1, 2, 3],
  sum: () => {
    return this.array.reduce((result, item) => result + item)
  }
}

//TypeError: Cannot read property 'array' of undefined
calculate.sum()
```

## conclusion

箭頭函式 tips

- 傳統函式縮寫且只有一個參數才不用加`括號 ()`。
- 特殊情況會造成 this 指向`改變`。
- 沒有 arguments 參數。
- 沒有 constructor、prototype。
- 沒有 Array 無法使用陣列方法。

## reference

- [[JS] 箭頭函式（arrow function）和它對 this 的影響](https://pjchender.dev/javascript/js-arrow-function/)
- [JavaScript基本功修練：Day21 - 箭頭函式](https://ithelp.ithome.com.tw/articles/10249940)
