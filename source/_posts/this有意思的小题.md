---
title: this有意思的小题
date: 2021-04-10 15:11:04
tags: [js, this]
categories: 
- js
---

 偶然发现的一道小题，原题：

```js
function a(xx){
  this.x = xx;
  return this
};
var x = a(5);
var y = a(6);

console.log(x.x)  // undefined
console.log(y.x)  // 6
```

刚开始没明白为啥是这个结果，最后才发现这个问题有点坑

```js
function a(xx){
  this.x = xx;
  console.log(this)
  return this
};
var x = a(5);
var y = a(6);

console.log(x)  // 6
console.log(y)  // window
```

`y=a(6)`未执行的时候，x是`window`

当`y=a(6)`执行的时候，会将`window.x`重新赋值，所以打印`x.x=undefined`