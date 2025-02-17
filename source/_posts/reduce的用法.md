---
layout: js方法reduce实践.md
title: reduce的用法
date: 2021-07-17 10:31:12
tags: [js]
categories: 
- js
- es6
---

目的：使用reduce将一个数组，根据元素的某个属性进行合并
存在数组如下：

<!-- more -->

```js
let arr = [
    {
        id: 1,
        num: 2,
    },
    {
        id: 2,
        num: 3,
    },
    {
        id: 3,
        num: 4,
    },
    {
        id: 1,
        num: 3,
    },
];
```
现在需要将id相同的进行合并，并且累计num
实现方案如下
```js
let newArr = arr.reduce((prev, cur) => {
	//prev是新数组，cur是当前元素
    let ids = prev.map((item) => {
    	return item.id;
    });
    if (ids.includes(cur.id)) {
      	prev = prev.map((item) => {
          if (item.id == cur.id) {
              item.num += cur.num;
          }
          return item;
      });
    } else {
    	prev.push(cur);
    }
    return prev;
}, []);
console.log(newArr);
```
之后就发现，id相同的合并累加了。
**注意点：此方法在vue中，一般使用场景会在watch中，切记，在累加数量的时候要深拷贝赋值，否则会陷入无限循环！！！**

