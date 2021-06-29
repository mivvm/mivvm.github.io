---
title: call apply bind实现原理
date: 2020-11-29 10:55:28
tags: [js, this, call, apply, bind]
categories: 
- js
---

1. call

   实现步骤：

   + 判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
   + 判断传入上下文对象是否存在，如果不存在，则设置为 window
   + 处理传入的参数，截取第一个参数后的所有参数。
   + 将调用函数作为上下文对象的一个属性。
   + 使用上下文对象来调用这个方法，并保存返回结果。
   + 删除刚才新增的属性。
   + 返回结果。

```js
Function.prototype.call = function(context) {
    if (typeof this !== "function") {
        throw new TypeError("Error")
    }
    let result = null
    // 判断 context 是否传入，如果未传入则设置为 window
    context = context || window
    // 将调用函数设为对象的方法
    context.fn = this
    let args = [...arguments].splice(1) //获取参数
    result = context.fn(...args)
    delete context.fn
    return result
}
```

2. apply

   实现步骤：

   - 判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
   - 判断传入上下文对象是否存在，如果不存在，则设置为 window
   - 将调用函数作为上下文对象的一个属性。
   - 判断参数值是否传入
   - 使用上下文对象来调用这个方法，并保存返回结果。
   - 删除刚才新增的属性。
   - 返回结果。

```js
Function.prototype.apply = function(context) {
    if (typeof this !== "function") {
        throw new TypeError("Error")
    }
    let result = null
    context = context || window
    context.fn = this
    if(arguments[1]) {
        result = context.fn(...arguments[1])
    }else {
        result = context.fn()
    }
    delete context.fn
    return result
}
```

3. bind

   实现步骤：

   + 判断调用对象是否为函数，即使是定义在函数的原型上的，但是可能出现使用 call 等方式调用的情况。
   + 保存当前函数的引用，获取其余传入参数值。
   + 创建一个函数返回
   + 函数内部使用 apply 来绑定函数调用，需要判断函数作为构造函数的情况，这个时候需要传入当前函数的 this 给 apply 调用，其余情况都传入指定的上下文对象。

```js
Function.prototype.bind = function(context) {
    if (typeof this !== "function") {
        throw new TypeError("Error")
    }
    let result = null
    context = context || window
    fn = this
    let args = [...arguments].splice(1) //获取参数
    return function Fn() {
        return fn.apply(
            this instanceof Fn ? this : context,
            args.concat(...arguments)
        )
    }
}
```