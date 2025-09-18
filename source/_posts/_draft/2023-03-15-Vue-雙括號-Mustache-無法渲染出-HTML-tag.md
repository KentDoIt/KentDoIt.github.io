---
title: Vue 雙括號 Mustache 無法渲染出 HTML tag
categories: Vue
tags:
  - Vue
description: 此篇會探討為什麼雙括號 Mustache 無法渲染出 HTML tag 的問題
abbrlink: 3454538903
date: 2023-03-15 16:55:12
---

## 科普 Mustache

- 使用方式：`{{ }} 雙括號`
- 用途：動態顯示 data 以及綁定

> 支持兩種參數

Inside text interpolations 文本插值
- 最基本的數據綁定方式
```html
<span>Message: {{ msg }}</span>
```

expression 表達式
- 皆會轉換成 JavaScript expressions
```html
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div :id="`list-${id}`"></div>
```

補充：vue templates 中可以使用 JavaScript expressions 的兩種時機
- Inside text interpolations 使用 `{{}} mustaches`
- 任何使用 `v-` 開頭的屬性值（例如：v-if、v-bind）

{% note default %}
以上屬於對 Mustache 的介紹，下方開始探討為什麼無法渲染出 HTML tag。
{% endnote %}

---

## 問題探討

Q：為什麼無法渲染出 HTML tag

A：[官方文件](https://vuejs.org/guide/essentials/template-syntax.html#raw-html)中有提到「mustaches interpret the data as plain text」

![](https://imgur.com/wThhCkS.png)

代表 mustaches 渲染得結果都會作為純文字（string）渲染到畫面上，因此不會解析成 HTML tag。

- 文件中也有表示可以透過 [Raw HTML](https://vuejs.org/guide/essentials/template-syntax.html#raw-html)（v-html） 來渲染出 HTML tag。

{% note default %}
下方示範如何解決
{% endnote %}

---

## 解決方法

情境範例：依照 product.is_enabled 渲染出不同的 class、textContent

> 錯誤範例：因為渲染出來的是純文字，不會解析成 HTML tag。

```html
{{ product.is_enabled ? '<span class="text-success">啟用</span>' : '<span>未啟用</span>' }}
```

{% note default %}
因此如果渲染 HTML 需求，不能透過 Mustache 字串插值的方式，下方示範演示其他兩種解決方法。
{% endnote %}

> 三元運算子

- 相同類型的結構可以使用`三元運算子`動態切換不同的值，這邊 class 綁定的值屬於`陣列`。

```html
<span :class="[product.is_enabled ? 'text-success' : 'text-warning']">{{ product.is_enabled ? '啟用' : '未啟用' }}</span>
```

- 如果動態切換條件只需要判斷一種結果，則下方範例兩種形式都可以。

```html
<span :class="{'text-success': product.is_enabled, 'text-warning': !product.is_enabled}">{{ product.is_enabled ? '啟用' : '未啟用' }}</span>
<span :class="[{'text-success': product.is_enabled}, {'text-warning': !product.is_enabled}]">{{ product.is_enabled ? '啟用' : '未啟用' }}</span>
```

> v-html

- 透過三元運算子來決定要渲染的 HTML tag。

```html
<div v-html="product.is_enabled ? `<span class='text-success'>啟用</span>` : `<span class='text-warning'>未啟用</span>`"></div>
```

- data 定義一個變數來存放 html tag，再作為 v-html 綁定對象。

```html
const app = {
  data() {
    return {
			rawHtmlEnabledIsTrue: "<span class="text-success" v-if="product.is_enabled">啟用</span>",
			rawHtmlEnabledIsFalse: "<span class="text-warning" v-if="!product.is_enabled">未啟用</span>"
		}
	}
}
<div v-html="product.is_enabled ? rawHtmlEnabledIsTrue : rawHtmlEnabledIsFalse"></div>
```

{% note danger %}
提醒：這種動態渲染 HTML 方式，容易導致 XSS 漏洞，因此必須要在可信任的 content（例如上方範例的 rawHtmlEnabledIsTrue、rawHtmlEnabledIsFalse）中在使用 v-html，絕對不能用在使用者提供的內容上（例如：input）。
{% endnote %}

延伸問題
- ray [解決 Vue 渲染太慢而導致看到 Mustache (雙花括號)的問題](https://israynotarray.com/vue/20221202/2131448011/)
