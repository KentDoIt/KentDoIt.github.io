---
title: JS 淺拷貝（Shallow Copy）、深拷貝（Deep Copy）
categories: JavaScript
tags:
  - JavaScript
description: 此篇會介紹在幾種在 JS 實作淺拷貝、深拷貝的方法。
abbrlink: 1956994369
date: 2023-03-08 11:42:01
---

## 科普

> 快速科普一下什麼是傳值（Pass by value）、傳址（Pass by reference）

{% note default %}
依照資料型別來決定傳遞行為是 Pass by value 或 Pass by reference。
{% endnote %}

基本型別 (Primitive Type) 

- 就是傳值（value），會是分別獨立的記憶體空間。（也稱深拷貝）

物件型別 (Object Type) 

- 就是傳址（address），會指向同一個記憶體空間。（也稱淺拷貝）

{% note default %}
科普就到這邊，下方會用實際範例來演釋實作淺拷貝、深拷貝的方法。
{% endnote %}

---

## 淺拷貝（Shallow Copy）

> 最多只能複製第一層（淺層），其它層還是指向相同記憶體位置，因此在較深的結構賦予值還是會相互影響。


### 原生物件型別

```js
let obj1 = {name: 'kent'};
let obj2 = obj1;
console.log(obj1 === obj2);  // true，第一層
console.log(obj1.name === obj2.name);  // true，第一層
```

### 展開運算子

```js
let obj1 = {name: 'kent'};
let obj2 = {...obj1};
console.log(obj1 === obj2);  // false，第一層
console.log(obj1.name === obj2.name);  // true，第二層
```

### object assign

```js
let obj1 = {name: 'kent'};
let obj2 = Object.assign({},obj1);
console.log(obj1 === obj2);  // false，第一層
console.log(obj1.name === obj2.name);  // true，第二層
```

---
## 深拷貝（Deep Copy）

> 建立新的記憶體空間來複製全部內容（全部深度），因此兩者是獨立的記憶體空間，彼此不會相互影響。

### JSON.parse( JSON.stringify( object ) )
- `JSON.stringify()`：轉成 JSON 字串，`無法` 轉換為 JSON 字串的屬性則自動 `忽略`。
- `JSON.parse()`：解析 JSON，將 JSON 轉為基本型別 (Primitive Type) 、物件型別 (Object Type) 。

```js
var obj = {
	 name: 'kent', 
   age: 5, 
   action: function(){
     console.log('function')      
   },
   religion: undefined,
   fortune: NaN
};
var obj2 = JSON.parse( JSON.stringify(obj));
//function、undefined 不是 JSON 有效值，且 NaN 被轉換成 null
console.log(obj2);
```

### lodash 套件的 cloneDeep()
- `clone()`：只複製淺層的值。
- `cloneDeep()`：透過遞迴的方式複製全部的值。
- 好處：不會遇到 `JSON.stringify/parse` 部分值會非預期改變的問題。

```js
import { clone, cloneDeep } from 'lodash';
let obj1 = {
	name: 'kent', 
	age: 5, 
};
let obj2 = clone(obj1);
let obj3 = cloneDeep(obj1);
console.log(obj1.name === obj2.name); // true，淺拷貝
console.log(obj1.name === obj3.name); // false，深拷貝
```
---
## 三種自製簡單遞迴 function 作法

第一種：
```js src: https://medium.com/coding-hot-pot/javascript中的深拷貝與淺拷貝-9493a5153d16 JavaScript中的深拷貝與淺拷貝
const deepCopy = (val) => {
	let newVal = Array.isArray(val) ? [] : {};
	for (let v in val) {
	    if (val.hasOwnProperty(v)) {
	        if ('object' === typeof val[v]) {
	            newVal[v] = deepCopy(val[v]);
	        } else {
	            newVal[v] = val[v];
	        }
	    }
	}
	return newVal;
};

const originalData = {
  firstLayerNum: 10,
  obj: {
    secondLayerNum: 100,
  },
};
const clonedData = deepCopy(originalData);

console.log(clonedData === originalData); // false，第一層
console.log(clonedData.obj.secondLayerNum === originalData.obj.secondLayerNum); // false，第二層
```

第二種：
```js src: https://www.programfarmer.com/articles/javaScript/javascript-shallow-copy-deep-copy JS 中的淺拷貝 (Shallow copy) 與深拷貝 (Deep copy) 原理與實作
function deepCopy(inputObject) {
  // Return the value if inputObject is not an Object data
  // Need to notice typeof null is 'object'
  if (typeof inputObject !== 'object' || inputObject === null) {
    return inputObject;
  }

  // Create an array or object to hold the values
  const outputObject = Array.isArray(inputObject) ? [] : {};

  // Recursively deep copy for nested objects, including arrays
  for (let key in inputObject) {
    const value = inputObject[key];
    outputObject[key] = deepCopyFunction(value);
  }

  return outputObject;
}

const originalData = {
  firstLayerNum: 10,
  obj: {
    secondLayerNum: 100,
  },
};
const clonedData = deepCopy(originalData);

console.log(clonedData === originalData); // false，第一層
console.log(clonedData.obj.secondLayerNum === originalData.obj.secondLayerNum); // false，第二層
```

第三種：
```js src: https://jsbench.me/euk9l6x9jx/1 jsbench example
function deepCopy(obj) {
    var copy;

    // Handle the 3 simple types, and null or undefined
    if (null == obj || "object" != typeof obj) return obj;

    // Handle Date
    if (obj instanceof Date) {
        copy = new Date();
        copy.setTime(obj.getTime());
        return copy;
    }

    // Handle Array
    if (obj instanceof Array) {
        copy = [];
        for (var i = 0, len = obj.length; i < len; i++) {
            copy[i] = deepCopy(obj[i]);
        }
        return copy;
    }

    // Handle Object
    if (obj instanceof Object) {
        copy = {};
        for (var attr in obj) {
            if (obj.hasOwnProperty(attr)) copy[attr] = deepCopy(obj[attr]);
        }
        return copy;
    }

    throw new Error("Unable to copy obj! Its type isn't supported.");
}

const originalData = {
  firstLayerNum: 10,
  obj: {
    secondLayerNum: 100,
  },
};
const clonedData = deepCopy(originalData);

console.log(clonedData === originalData); // false，第一層
console.log(clonedData.obj.secondLayerNum === originalData.obj.secondLayerNum); // false，第二層
```

陣列延伸閱讀請參考：
- askie 大：[透過複製陣列理解 JS 的淺拷貝與深拷貝 - JavaScript](https://askie.today/javascript-deep-copy-swallow-copy/)

---
## performance

以下圖片出自於 StackOverflow [How do I correctly clone a JavaScript object?](https://stackoverflow.com/questions/728360/how-do-i-correctly-clone-a-javascript-object/61523278#61523278) 討論中提到上述淺拷貝、深拷貝實作方法的在三大瀏覽器中的效能比較。

> 淺拷貝：展開運算子、assign 兩者是差不多的優異

![](https://imgur.com/SsrJiev.jpg)

> 深拷貝：遞迴 function > lodash > JSON

![](https://imgur.com/QcWxZ4C.jpg)

---
## conclusion

{% note default %}
從上方效能比較圖看起來，使用 `JSON.parse(JSON.stringify( ))` 方法效能是比較差的，此外的方法會依照需求做調整。
{% endnote %}
---
# reference

- 簡單明瞭：[JavaScript中的深拷貝與淺拷貝](https://medium.com/coding-hot-pot/javascript%E4%B8%AD%E7%9A%84%E6%B7%B1%E6%8B%B7%E8%B2%9D%E8%88%87%E6%B7%BA%E6%8B%B7%E8%B2%9D-9493a5153d16)
- [JS 中的淺拷貝 (Shadow copy) 與深拷貝 (Deep copy) 原理與實作](https://www.programfarmer.com/articles/javaScript/javascript-shallow-copy-deep-copy)