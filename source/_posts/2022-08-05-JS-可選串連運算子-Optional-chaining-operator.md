---
title: JS 可選串連運算子 Optional chaining operator
categories: JavaScript
tags:
  - ES6
  - JavaScript
description: 此篇學習在 JS 中如何透過這個 ES2020 新增的語法糖可選串連運算子，來避免遇到物件屬性不存在所導致的報錯。
abbrlink: 3900271525
date: 2022-06-28 17:03:08
---

## 可選串連運算子 Optional chaining operator

{% note primary %}
[MDN](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Operators/Optional_chaining)：
可選串連運算子 **`?.`** 允許進行深層次的物件值存取，而無需透過明確的物件值串連驗證。`?.` 運算子的操作與 `.` 屬性存取運算子相似，後者會在參照到 [nullish (en-US)](https://developer.mozilla.org/en-US/docs/Glossary/Nullish) 的值為 [null](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/null) or [undefined](https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/undefined) 來判斷當不存在時會出現錯誤，而前者可選串連則回傳 `undefined`。 當需要存取一個函數，而這函數並不存在時，則會回傳 `undefined`。
{% endnote %}

{% note default %}
白話文：存取一層以上的物件屬性時，就算屬性不存在也不會報錯，而是回傳 undefined。
{% endnote %}

語法：在 `.` 前方加上 `?`。

{% codeblock lang:shell line_number:false %}
obj?.prop
obj?.[expr]
arr?.[index]
func?.(args)
{% endcodeblock %}

> Tips

- 當 `?.` 左側屬性不存在，回傳 `undefined` 而非 TypeError。

{% note primary %}
物件特性：取用未定義屬性則會回傳 undefined，但僅限一層！不然會噴 TypeError。
（因此有了可選串連運算子的誕生）
{% endnote %}

### 語法差異

{% note info %}
舊方法：手動判斷 typeof 是否為 undefined。
新方法：直接在屬性 `.` 前方加上 `?`。
{% endnote %}

{% note primary %}
判斷是否存在會依照 [nullish value](https://developer.mozilla.org/en-US/docs/Glossary/Nullish) （null、undefined）直來判斷，若值為 nullish value 就表示不存在。
{% endnote %}

```js e.g. 新舊寫法差異
let school = {
  info: {
    student: {
      name: 'kent',
      age: 18,
    },
  },
};

// TypeError: Cannot read properties of undefined (reading 'name')
let name = school.info.teacher.name; 

// 舊方法
if (typeof (school.info.teacher) !== 'undefined'){
  let name = school.info.teacher.name;
}

// 新方法
let name = school.info.teacher?.name;
```

{% note default %}
下方會用更多範例來介紹，範例中會使用[短路求值](https://kentdoit.github.io/javascript/350805089/)的方式來演示，還不暸解的朋友可以參考這篇文章。
{% endnote %}

---

## 情境範例

### 判斷物件屬性是否存在

- 舊方法：`&&`
  - 會先判斷上一層屬性是否存在，`&& 左邊` 是否為 nullish value （null 或 undefined）
  - 不是null 或 undefined 才繼續執行 `&& 右邊`。
- 新方法：`?.`
  - 當 `?.` 左側屬性是否為 nullish value （null 或 undefined），則回傳 undefined。

```js
let school = {
  info: {
    student: {
      name: 'kent',
      age: 18,
    },
  },
};

// 問題
let name = school.info.teacher.name; // TypeError: Cannot read properties of undefined (reading 'name')

// 舊方法
let name = school.info.teacher && school.info.teacher.name;   // undefined

// 新方法
let name = school.info.teacher?.name;      // undefined
let name = school.info['teacher']?.name;   // undefined
```

## 延伸應用

### 預防方法不存在

```js
let obj = {
  fn: function () {
    console.log('fn');
  }
};

console.log(obj.fnn());   // TypeError: obj.fnn is not a function
console.log(obj.fnn?.()); // undefined
```

### 結合空值合併運算符（??）來設置預設值

```js 延續上方的程式碼
let name = school.info.teacher?.name ?? '陳怡君';
```

## conclusion

{% note default %}
比較直接透過錯誤訊息判斷問題（難追尋問題原因），但如果觀念夠的話，還是可以透過邏輯拆解一步一步尋找。
{% endnote %}

## reference

- [我所不知道的javascript #7](https://github.com/HelloJunWei/blog/issues/7)

延伸閱讀

- 程式猿吃香蕉：[前端開發 🦏 來談 JavaScript 的 Optional Chaining 和 Nullish Coalescing (一)](https://medium.com/%E7%A8%8B%E5%BC%8F%E7%8C%BF%E5%90%83%E9%A6%99%E8%95%89/%E4%BE%86%E8%AB%87-javascript-%E7%9A%84-optional-chaining-%E5%92%8C-nullish-coalescing-part-i-992625a1861d)
