---
layout: js
title: js向一个数组中特定位置添加另一个数组
date: 2021-09-26 10:57:59
tags: [js]
categories: 
- js

---

js实现向一个数组中特定位置添加另一个数组

<!-- more -->

```js
let arr1 = [1, 5, 6, 7];
let arr2 = [2, 3, 4];
let index = 1; // 要插入的位置

Array.prototype.splice.apply(arr1, [index, 0].concat(arr2)); // 使用 apply 方法将 arr2 数组的元素作为参数传递给 splice 方法

console.log(arr1); // 输出 [1, 2, 3, 4, 5, 6, 7]

```
`splice方法`只能单独添加或者删除，这时候可以通过`apply`方法将参数有一个改为多个，就可以实现向另一个数组中特定位置添加另一个数组的功能。
