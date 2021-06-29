---
title: call apply bind改变this指向
date: 2020-11-09 10:49:51
tags: [js, this, call, apply, bind]
categories: 
- js
---

### call和apply的区别

它们的作用一模一样，区别仅在于传入参数的形式的不同。

- `apply `接受两个参数，第一个参数指定了函数体内 `this `对象的指向，第二个参数为一个带下标的集合，这个集合可以为数组，也可以为类数组，`apply `方法把这个集合中的元素作为参数传递给被调用的函数。
- `call `传入的参数数量不固定，跟 `apply `相同的是，第一个参数也是代表函数体内的 `this `指向，从第二个参数开始往后，每个参数被依次传入函数。

### bind

+ 与两者不同的是返回结果是一个函数
+ 与call的传参方式一样

```js
var obj = {
    name: '张三'
}
var name = '李四'
var age = 20
var year = 10
function s(age, year) {
    console.log(this.name + '今年' + age + '毕业' + year + '年')
}
//直接执行
s(age, year)  //李四今年20，毕业10年
s.call(obj,30,5)  //张三今年30毕业5年
s.apply(obj,[30,5])  //张三今年30毕业5年
s.bind(obj,30,5)()  //张三今年30毕业5年
```

这三种方式到底是怎么实现改变`this`指向的呢？见[此章节](/2020/11/29/call-apply-bind实现原理/)