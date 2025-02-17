---
layout: for循环内部使用splice.md
title: for循环内部使用splice
date: 2021-06-28 10:19:36
tags: [js]
categories: 
- js
---

for循环内部使用splice，发现有些元素并没有遍历，出现这个的问题原因是因为，splice会修改原数组的长度，导致下标发生变化。
解决方法1：

```js
var data = [1,2,3,4,5,6,7]
for(var i = data.length - 1; i >=0; i--) {
   if(data[i] > 5) {
       data.splice(i, 1)
   }
}
```
使用倒序，可以**完美**解决掉这种情况
解决方法2：

```js
var data = [1,2,3,4,5,6,7]
for(var i = 0; i < data.length;) {
    if(data[i] > 5) {
        data.splice(i, 1)
    }else {
        i++
    }
}
```
当数组长度发生变化的时候，不改变下标
解决方法3：
深拷贝原数组，遍历新数组，操作原数组