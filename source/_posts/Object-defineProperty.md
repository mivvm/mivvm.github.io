---
title: Object.defineProperty
date: 2019-11-03 11:14:01
tags: [js, js数据拦截]
categories: 
- js
- vue
---

参数：obj,prop,descriptor

对象里目前存在的属性描述符有两种主要形式,**数据描述符和存取描述符**，数据描述符是一个具有值的属性，该值可以是可写的，也可以是不可写的,存取描述符是由getter函数和setter函数所描述的属性。一个描述符只能是这两者其中之一

### 两种描述符共享键值

+ configurable：值为true，该属性的`描述符的属性`才能被改变，`对象的属性`被从对应的对象上删除，默认false

  + 默认值

    ```js
    var o = {};
    Object.defineProperty(o, 'a', {
        get() { return 1; },
        configurable: false
    });
    Object.defineProperty(o, 'a', {
      	configurable: true
    }); // throws a TypeError
    Object.defineProperty(o, 'a', {
        enumerable: true
    }); // throws a TypeError
    ```

  + true

    ```js
  var o = {};
    Object.defineProperty(o, 'a', {
        get() { return 1; },
        configurable: true
    });
    Object.defineProperty(o, 'a', {
      	configurable: true
    }); // throws a TypeError
    
    Object.defineProperty(o, 'a', {
        enumerable: true
    }); // throws a TypeError
    console.log(o)
    ```
  
    

  

+ enumerable：值为true:，该属性才会出现在对象的枚举属性中，默认false

  [`可枚举属性见`](/2019/11/04/可枚举属性/)

  ```js
  var obj = {}
  Object.defineProperty(obj, 'a', {
      value: 1,
      enumerable: true
  })
  Object.defineProperty(obj, 'b', {
      value: 2,
      enumerable: true
  })
  Object.defineProperty(obj, 'c', {
      value: 3,
      enumerable: false
  })
  obj.d = 4; // 如果使用直接赋值的方式创建对象的属性，则 enumerable 为 true
  Object.defineProperty(obj, Symbol.for('e'), {
      value: 5,
      enumerable: true
  });
  for(var x in obj) {
      console.log(x)  //a,b,d
  }
  Object.keys(obj)  //["a", "b", "d"]
  obj.propertyIsEnumerable('a')  //true
  obj.propertyIsEnumerable('c')  //false
  //propertyIsEnumerable 方法可以判断某个属性是否被枚举
  var p = { ...obj }  //扩展运算符针对enumerable 为 true的进行计算w
  console.log(p)  //{a: 1, b: 2, d: 4, Symbol(e): 5}
  ```

  

### 数据描述符具有可选键值

+ value：该属性对应的值，具有任何有效的js值

+ writable：值为true时，也就是上面的value才能被赋值，默认false

  ```js
  var obj = {}
  Object.defineProperty(obj, 'a', {
      value: 1,
      writable: false
  })
  obj.a = 2
  console.log(obj)  // {a: 1}
  ```

### 存取描述符具有可选键值

+ get：函数，访问该属性会调用此函数，执行不传入任何参数，但是会传入**this**。该函数返回值会被用作属性的赋值，默认**undefined**

  `注意：使用get方法，要提前定义一个变量，这个变量就是会被用作属性的值`

+ set：函数，当属性被修改时，会调用此函数。该方法接受一个参数，会传入赋值时的**this**对象，默认**undefined**

  ```js
  var obj = {}
  Object.defineProperty(obj, 'a', {
      value: 1,
      writable: false,
      configurable: true
  })
  //只有当  configurable为true才能修改描述符
  var bValue = 38;
  Object.defineProperty(obj, 'a', {
      configurable: true,
      get: function() {
          console.log(this)  //{}
          return bValue
      },
      set: function(newValue) {
          console.log(this)  //{}
          bValue = newValue
          console.log(newValue) //9
          console.log(this)  //{}
      }
  })
  obj.a = 9   //会调用set方法  
  console.log(obj.a)  //会调用get方法  //9
  ```

  

如果一个描述符不具有 `value`、`writable`、`get` 和 `set` 中的任意一个键，那么它将被认为是一个数据描述符。如果一个描述符同时拥有 `value` 或 `writable` 和 `get` 或 `set` 键，则会产生一个异常。

记住，这些选项不一定是自身属性，也要考虑继承来的属性。为了确认保留这些默认值，在设置之前，可能要冻结 [`Object.prototype`]，明确指定所有的选项，或者通过`Object.create(null)`将`__proto__`属性指向**null**

```
var obj = {}
var descriptor = Object.create(null);
console.log(descriptor) // {} 无属性
Object.defineProperty(obj, 'key', descriptor);
```