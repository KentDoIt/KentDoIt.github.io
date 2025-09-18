---
title: JS 解構賦值 Destructuring Assignment
abbrlink: 4089638591
date: 2022-07-10 17:03:44
categories: JavaScript
tags:
  - ES6
  - JavaScript
description: 此篇會介紹 JS ES6 新增的語法解構賦值的概念以及如何使用。
---

## 解構賦值 Destructuring Assignment

{% note primary %}
[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)：屬於表達式，可以將陣列、物件解構（拆解結構，Destructure），透過更簡潔的語法來取值陣列、物件到對應的變數上。
{% endnote %}

{% note default %}
解構的過程就像是鏡子映射，將右邊的值映射到左邊變數上，則沒有映射到的就是 `undefined`。
{% endnote %}

語法：

- 使用 `{}`、`[]` 來解構，並依照解構的對象型別來選擇是 `{}`、`[]`。
- 若無法對應到的值為 undefined。

```js e.g. 物件、陣列基礎的解構範例
const obj = { name: 'kent' };
const arr = [20, 22];

// 舊寫法
const a = arr[0];
const b = arr[1];
const name = obj.name;
const age = obj.age;

// 新寫法
const [a, b] = arr // a=20, b=22
const { name, age } = obj; // name='kent', age=undefined
```

{% note default %}
下方會透過陣列、物件、函式三種的解構賦值範例來介紹其特性以及應用。
{% endnote %}

---

### 陣列解構

![](https://imgur.com/qMguOpU.png)

> Tips

- 沒有對應到的值為 undefined。

```js
let [a, b, c, d] = [1, 2, 3];
console.log(a, b, c, d);     // a=1, b=2, c=3, d=undefined
```

- 有順序性，會依照索引值順序來對應到左邊變數上，長度不夠的索引值會自動全部忽略（因此需要透過空變數，來跳過特定陣列元素）。

```js
let [a, b, c, d] = [1, 2, 3, 4];
console.log(a, b, c, d);   // a=1, b=2, c=3, d=4
```

- 使用空變數來跳過該索引值。

```js
// 切記陣列有順序性，是依照索引值來賦予值，因此 d 為 3
let [a, , d] = [1, 2, 3, 4];
console.log(a, d);      // a=1, d=3
```

> 注意事項

{% note default %}
陣列解構賦值如果結尾不加分號會噴 Error。
{% endnote %}

{% note primary %}
[standardJS](https://standardjs.com/readme-zhtw.html) 中有提到，絕對不要使用 `(` 、 `[` 或 ` `` ` 當開頭，因為會造成無法自動補上分號，一段加了 `Enter 鍵(\n)`後，JS 剖析器會在執行期間自動插入分號。
{% endnote %}

```js
let a = 0
let b = 0
console.log(a, b)
[a, b] = [20, 22] // TypeError: Cannot set property '0' of undefined
```

解決方法

- 在使用 ( 、 [ 或 ` 當開頭前面加上分號。

```js
let a = 0
let b = 0
console.log(a, b);  // 兩者都可以
;[a, b] = [20, 22]  // 兩者都可以
```

> 常見應用

#### 字串拆解

```js
// 舊寫法
let arr = '世界和平'.split('');
let a = arr[0];
let b = arr[1];
let c = arr[2];
let d = arr[3];
console.log(a, b, c, d)      // a=世, b=界, c=和, d=平

// 新寫法
let [a, b, c, d] = '世界和平';
console.log(a, b, c, d)      // a=世, b=界, c=和, d=平
```

#### 變數互換

```js
let change;
let name1 = 'kent';
let name2 = 'ken';

// 舊寫法
change = name1;
name2 = change;
console.log(name1, name2); // name1=ken, name2=kent

// 新寫法
[name1, name2] = [name2, name1];
console.log(name1, name2); // name1=ken, name2=kent
```

---

### 物件解構

![](https://imgur.com/DuSVx4i.png)

> Tips

- 沒有對應到的值為 undefined。

```js
let { name, age } = { name: 'kent' };
console.log(name, age);   // name='kent', age=undefined
```

- 沒有順序性，左右兩邊的數量不相等也沒問題，但無法使用空白變數（否則會噴 Error）。

```js
let { age, sex } = { name: 'kent', age: 18, sex: 'M' };
console.log(age, sex);   // name='kent', sex='M'

let { age, ,sex } = { name: 'kent', age: 18, sex: 'M' };
console.log(age, sex);   // SyntaxError: Unexpected token ','
```

- 直接使用 {}，需要在外面加上 ()，否則會噴 Error 被視為區塊（block）而非物件。

```js
// 錯誤寫法
{ age, name } = { name: 'kent', age: 20 };
console.log(name, age);   // SyntaxError: Unexpected token '='

// 正確寫法
({ age, name } = { name: 'kent', age: 20 });
console.log(name, age);   // {name: 'kent', age: 20}
```

> 常見應用

#### 重複指定

- 但都指向相同位置並使用[傳址（ by Reference）](https://kentdoit.github.io/javascript/3842658283/)方式，因此只要更改一個變數，其他變數值也會受影響。

```js
let obj = { name: 'kent' };

// 舊寫法
let name2 = name1 = obj;
console.log(name1, name2);  // name1=kent, name2=kent

// 新瀉法
let { name: name1, name: name2} = obj;
console.log(name1, name2);  // name1=kent, name2=kent
```

#### 重新賦予變數名稱

- 解構變數後方加上 : 賦予新的變數名稱。

```js
let obj = { name: 'kent', age: 18 };

// 舊寫法
let nickName = obj.name;
let age = obj.age;
console.log(nickName, age); // name='kent', age=18

// 新寫法
let { name: nickName, age} = obj;
console.log(nickName, age); // name='kent', age=18
```

{% note default %}
推測重新賦予名稱的方法，和 Object Literal 縮寫特性相關，因此原本的解構寫法應該是 `{ name: name } = { name: 'kent' };` 只是因為縮寫特性讓我們寫 `{ name } = { name: 'kent' };` 即可，想暸解更多 Object Literal 特性可以參考這篇[文章](https://kentdoit.github.io/javascript/2900651289/)。
{% endnote %}

#### 快速提取 JSON 資料

```js
let JSON_data = {
  id: 1237987,
  name: 'kent',
  age: 18,
  sex: 'M',
  food: {
    jp: 'sushi',
    us: 'fries'
  },
};

// 舊寫法
function getFoodOld(food) {
  console.log(food);
}
getFoodOld(JSON_data.food);  // {jp: 'sushi', us: 'fries'}

// 新寫法
function getFoodNew({food}) {
  console.log(food);
}
getFoodOld(JSON_data);  // {jp: 'sushi', us: 'fries'}
```

---

### 函式參數解構

![](https://imgur.com/meYPIyC.png)

{% note default %}
函式參數的用法和一般解構一樣，只是從 `等號左邊解構到右邊` 變為 `arguments 解構到 parameters`。與陣列解構、物件解構沒有太大差異這邊就不多贅述。
下方用兩個範例分別演示兩種函式參數解構的新舊寫法差異。
{% endnote %}

```js 函式陣列參數解構
// 舊寫法
function fn (args) {
  let month = args[0];
  let day = args[1];
  console.log(month, day);   // month=11, day=11
}
fn([11, 11]);

// 新寫法
function fn ([month, day]) {
  console.log(month, day);   // month=11, day=11
}
fn([11, 11 ]);
```

```js 函式物件參數解構
// 舊寫法
function fn (args) {
  let month = args.month;
  let day = args.day;
  console.log(month, day);   // month=11, day=11
}
fn({ month: 11, day: 11 });

// 新寫法
function fn ({ month, day }) {
  console.log(month, day);   // month=11, day=11
}
fn({ month: 11, day: 11 });
```

{% note default %}
介紹到這邊對於解構賦值基礎運用已具備一定程度的理解，下方會介紹幾種稍微進階一咪咪的解構賦值應用。
{% endnote %}

---

### 非基礎應用

#### 設定默認值（預設值）

- 防止出現 undefined 結果，再解構變數後方使用 = 並賦予一個值。

```js
const obj = { name: 'kent' };
const arr = [20, 22];

// 陣列解構預設值
const [a, b, c = 1] = arr;
console.log(a, b, c);   // a=20, b=22, c=1

// 物件解構預設值
const { name, age = 18 } = obj;
console.log(name, age); // name='kent', age=18

// 函式參數解構預設值
function fn ([a, b, c = 1]) {
  console.log(a, b, c);   // a=20, b=22, c=1
}
fn(arr);
```

{% note danger %}
常見問題：函式參數為空會噴 Error
{% endnote %}

{% note default %}
解決辦法：對參數設預設值
{% endnote %}

```js
// 錯誤
function fn ([a, b, c = 1]) {
  console.log(a, b, c);   // TypeError: undefined is not iterable (cannot read property Symbol(Symbol.iterator))
}
fn();

// 正確
function fn ([a, b, c = 1] = []) {
  console.log(a, b, c);   // undefined undefined 1
}
fn(); // 若上面的錯誤範例傳入 fn([]) 則結果會相同
```

#### 巢狀解構

- 子解構，透過重新賦予的特性，將其在進行解構一次。

![](https://imgur.com/uXl2DrA.png)

```js
// 子陣列解構
let { name: boy, name: [men] } = { name: ['kent'], age: 18 };
console.log(boy, men);  //['kent'], 'kent'

// 子物件解構
let { info, info: {name} } = { info: {name: 'kent', age: 18} };
console.log(info, name);  //{name: 'kent', age: 18}, 'kent'
```

#### 巢狀預設值

- 對於需要依照不同情況組合不同參數非常方便（例如：config 參數）。

```js src: https://cythilya.github.io/2018/11/05/syntax/ 你懂 JavaScript 嗎？#29 語法（Syntax）
let defaults = {
  setupFiles: ['setup.js', 'setUp-another.js'],
  testUrl: 'https://sample.com.tw',
  support: {
    a: true,
    b: false,
  },
};

let config = {
  testUrl: 'https://sample-special.com.tw',
  support: {
    a: false,
    c: true,
  },
};

let dev_config = {
  setupFiles = defaults.setupFiles,
  testUrl = defaults.testUrl,
  support = defaults.support,
} = config;

console.log(dev_config);
```

#### 展開運算子、其餘運算子

- 這邊範例只會展示和解構相關，若想暸解關於展開運算子、其餘運算子可以參考這篇[文章](https://kentdoit.github.io/javascript/1214648668/)。

```js 展開運算子（組合資料）
// 陣列解構
let [a, ...b] = ['世', '界', '和', '平']; 
console.log(a, b); // a='世', b=['界', '和', '平']

// 對應不到的時候就是空陣列。
const [a, ...b] = ['世'];
console.log(a, b); // a='世', b=[]

// 物件解構
let { name, ...info } = { name: 'kent', age: 18, sex: 'M'};
console.log(name, info); // name='kent', info={age: 18, sex: 'M'}
```

```js e.g. 其餘參數（拆解資料）
function fn(...[a, b, c, d]) {
  console.log(a, b, c, d);
}

fn('世'); // '世' undefined undefined undefined
fn('世', '界', '和', '平');  // '世', '界', '和', '平'
```

{% note default %}
想看更多解構賦值應用請參考：[阮一峰-ECMAScript 6 入門](http://es6.ruanyifeng.com/#docs/destructuring)
{% endnote %}

---

## reference

- PJ 大：[[筆記] JavaScript ES6 中的陣列解構賦值（array destructuring）](https://pjchender.blogspot.com/2017/01/es6-array-destructuring.html)
- summer 大：[你懂 JavaScript 嗎？#29 語法（Syntax）](https://cythilya.github.io/2018/11/05/syntax/)
