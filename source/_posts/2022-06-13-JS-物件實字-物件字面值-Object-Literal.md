---
title: JS 物件實字 | 物件字面值 | Object Literal
categories: JavaScript
tags:
  - 物件
  - ES6
  - JavaScript
description: 此篇會介紹 物件實字 Object Literal 的作用以及特性。
abbrlink: 2900651289
date: 2022-06-13 10:53:10
---

## Literal

> [MDN](https://developer.mozilla.org/en-US/docs/Glossary/Literal) 定義：**Literals** represent values in JavaScript. These are fixed values—not variables—that you *literally* provide in your script.

{% note primary %}
Literals 在 JavaScript 代表固定的值（Fixed values）而不是變數的值。
{% endnote %}

JS 提供六種 Literal 類型

- [Array literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#array_literals)
- [Boolean literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#boolean_literals)
- [Floating-point literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#floating-point_literals)
- [Numeric literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#numeric_literals)
- [Object literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#object_literals)
- [RegExp literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#regexp_literals)
- [String literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#string_literals)

{% note default %}
下方只針對 `物件實字 Object Literal` 來做介紹以及範例。
{% endnote %}

---

## 物件實字（物件字面值） Object Literal

{% note default %}
使用 `大括號{}` 來建立物件的方式，就稱為 `物件實字 (Object Literals)`。
{% endnote %}

> 物件用法 tips

- 用大括號 {}。
- 每一筆資料包含 key、value，在物件中 key 為`屬性 (Properties)` 、value 為 `對名值對 (name-value pairs)` 。
- 多筆資料用逗號（comma）隔開。
- 宣告完後，還是可以再增加 `屬性` 進去。

e.g. 基礎物件

```js
let obj = {
  name: 'kent',
  age: 5,
  born: '2017-05-12',
  showInfo: function (){
    return `${this.name} 今年 ${this.age} 歲`;
  },
}
```

{% note default %}
下方會介紹幾種 ES6 推出的新特性 Enhanced Object Literals，使物件實字寫起來更簡潔的寫法。 
{% endnote %}

## 物件屬性

{% note default %}
當 `屬性名稱` 和 `value 變數名稱` 相同時，只需要在括號中設置變數名稱即可。
{% endnote %}

```js
// ES5
let msg1 = {
  text: '我是誰'
};
console.log(msg1.text);

// ES6
let text = '我是誰';
// 變數名稱 text 會變為物件屬性名稱，變數的值會變成對應的值
let msg2 = {  // 縮寫前
  text: text
};
let msg2 = { text }; // 縮寫後
console.log(msg2.text);
```

## 物件函示

{% note default %}
省略了 `冒號 :` 和 `function 關鍵字`。
{% endnote %}

```js
// ES5
let obj1 = {
  fn: function() {
    return 'ES5';
  }
};
console.log(obj1.fn());
// 箭頭函式寫法
let obj2 = {
  fn: () => {
    // 取得到 arguments 就是傳統函式，否則為箭頭函式的話會噴 ReferenceError
    console.log(arguments); // ReferenceError
    return 'ES5';
  }
};
console.log(obj2.fn());

// ES6
let obj3 = {
  fn() {
    console.log(arguments); // Arguments ['arg', callee: ƒ, Symbol(Symbol.iterator): ƒ]
    return 'ES6';
  }
};
console.log(obj3.fn('arg'));
```

> 注意事項

- ES6 使用簡寫的方式，函式屬於不具名，因此無法使用箭頭函式。
- ES6 預設的屬性名稱會是字串的型態。
  - ex：`fn() { /*...*/}` 等同於 `'fn'() { /*...*/}`。

## 具運算性的屬性名稱

{% note default %}
ES6 中使用 `中括號[]` 將`表達式`作為屬性名稱，即可透過運算子來賦予屬性名稱。
{% endnote %}

```js
let obj = {
  ["na"+"me"]: 'kent'
}
console.log(obj.name); // kent
```

## Property names

### 物件鍵使用 .Dot 和 []Square Brackets 差異

```js
let obj = {};
obj.1 = 1;    // Uncaught SyntaxError: Unexpected number
obj."1" = 1;  // Uncaught SyntaxError: Unexpected string

obj[1] = 1;
obj["1"] = 2;

console.log(obj) // ?
console.log(typeof(Object.keys(obj)[0])) // ?
```

{% note primary %}
Property accessors [MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Property_Accessors#property_names/#Property%20names)：「Property names are string or Symbol. Any other value, including a number, is coerced to a string. This outputs 'value', since 1 is coerced into '1'.」
白話文：「屬性值型態要是 string or symbol，否則都會自動被轉為字串」
{% endnote %}

> diff

1. 上方範例因為 Property accessors 的特性，使物件訪問資料時使用 `.` Dot 的情況下會發生錯誤，而使用 `[]` Square Brackets 則不會有這個限制。
{% note default %}
[MDN Object literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#object_literals) 有提到當屬性名稱為「數字」或 「無效的 JavaScript 識別字」時就必需要使用 `引號（''、""）`且無法使用 `.Dot` 來做屬性訪問，取而代之的就是使用 `[]Square Brackets`。
{% endnote %}
2. obj[1]、obj["1"] 會訪問到同一筆資料，從 typeof 印出結果為 `string` 能證明 number 被自動轉型為 string 所以兩者會相等。

{% note danger %}
使用 `[]` Square Brackets 雖然沒有了型別的限制，但會導致就算使用到保留字也不會出錯。
{% endnote %}

```js
let obj = {
  true: 'True Boolean',
  false: 'False Boolean',
  null: 'Null',
  undefined: 'Undefined'
};

console.log(obj[true]);
console.log(obj.true);
console.log(obj[false]);
console.log(obj.false);
console.log(obj[null]);
console.log(obj.null);
console.log(obj[undefined]);
console.log(obj.undefined);
```

### 動態賦予屬性名稱

{% note default %}
ES6 中使用 `中括號[]` 動態來賦予屬性名稱，打破 Literal `固定的值（Fixed values）` 特性。
{% endnote %}

```js
let objKey = 'name';
let obj = {
  [objKey]: 'kent'
};
console.log(obj.name);  // kent
```

## conclusion

1. ES6 提升了物件實字語法的簡潔性和靈活性。
2. 不建議使用非字串、保留字作為屬性名稱（雖然不會出錯，但很有可能產生非預期的錯誤）。

## quiz

{% note info %}
最後用三個問題來結束這回合，大家可以試著回答看看。
{% endnote %}

<details>
  <summary>Q1：物件實字的屬性可以用數值嗎？</summary>
  <answer>A1：可以</answer>
<pre><code>let obj = {
  0: 'num'
};
console.log(obj[0]); // num
</code></pre>
</details>

<details>
  <summary>Q2：ES6 函式縮寫是傳統函式還是箭頭函式？</summary>
  <answer>A2：箭頭函式，因為有印出 Arguments 類陣列。</answer>
<pre><code>let obj = {
  fn() {
    console.log(arguments); // Arguments []
    return 'ES6';
  }
};
obj.fn();
</code></pre>
</details>

<details>
  <summary>Q3：ES6 可以動態設置屬性名稱嗎？</summary>
  <answer>A3：可以，使用中括號[]。</answer>
<pre><code>let objKey = 'name';
let obj = {
  [objKey]: 'kent'
};
console.log(obj.name);  // kent
</code></pre>
</details>

## reference

- PJ大大：[[筆記]JavaScript ES6 中的物件的擴展 object literal extension）](https://pjchender.blogspot.com/2017/01/es6-object-literal-extension.html)
- [你不可不知的 JavaScript 二三事#Day23：ES6 物件實字威力加強版 (Enhanced Object Literals)](https://ithelp.ithome.com.tw/articles/10208316)
- [物件字面值擴充功能](https://chupai.github.io/posts/200515_js_object_literal_extension/)
- [Can JavaScript Object Keys Be Numbers or Non-string Values?](https://www.becomebetterprogrammer.com/can-javasscript-object-keys-be-numbers-or-non-string-values/)
