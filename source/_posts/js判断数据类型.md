---
title: js判断数据类型
date: 2020-10-19 10:21:20
tags: [js]
categories: 
- js
---

+ **typeof**：可以判断数字，字符串，等基本数据类型和Symbol

  ```js
  typeof 'str'  //String
  typeof 1  //Number
  typeof true  //boolean
  typeof undefined  //undefined
  typeof null  //object
  typeof {a: 1}  //object
  typeof [1]  //object
  typeof function a() {} //function
  typeof Symbol() //symbol
  var s = new Set()
  typeof s //object
  var m = new Map()
  typeof m //object
  ```

+ **constructor**：因为`undefined`,`null`没有`constructor`属性，所以不能判断这两者。

  ```js
  'str'.constructor == String  //true
  1.constructor == Number  //true
  true.constructor == Boolean  //true
  var o = {a:1}
  o.constructor == Object //true
  [1].constructor == Array //true
  new Function().constructor == Function  //true
  Symbol().constructor == Symbol //true
  var s = new Set()
  s.constructor == Set  //true
  var m = new Map()
  m.constructor == Map  //true
  ```

  他有两个作用，一是判断数据的类型，二是对象实例通过 `constrcutor` 对象访问它的构造函数。需要注意，如果创建一个对象来改变它的原型，`constructor`就不能用来判断数据类型了 

  ```js
  function Fn(){};
   
  Fn.prototype = new Array();
   
  var f = new Fn();
   
  console.log(f.constructor === Fn);    // false
  console.log(f.constructor === Array); // true
  ```

  

+ **instanceof**：根据规定，所有引用类型都是Object的实例，**内部运行机制是判断在其原型链中能否找到该类型的原型** 

  ```js
  2 instanceof Number                 // false
  true instanceof Boolean         // false 
  'str' instanceof String                // false 
  function(){} instanceof Function       // true
  [] instanceof Array  //true
  ({}) instanceof Object //true
  
  ```

+ **Object.prototype.toString.call()**：

  原理：每个对象都有一个 `toString()` 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。默认情况下，`toString()` 方法被每个 `Object` 对象继承。如果此方法在自定义对象中未被覆盖，`toString()` 返回 "[object *type*]"，其中 `type` 是对象的类型 

  ```js
  var a = [1,2]
  a.toString()  //"1,2" 此时调用的方法是Array.prototype.toString
  ```

  虽然`Array`也继承`Object`，但是js在`Array.prototype`上重写了`toString`，而我们通过`toString.call(arr)`实际上是通过原型链调用了`Object.prototype.toString` 

  ```js
  Object.prototype.toString.call(true) //[object Boolean]
  Object.prototype.toString.call(1) //[object Number]
  Object.prototype.toString.call('str') //[object String]
  Object.prototype.toString.call(undefined)  //[object Undefined]
  Object.prototype.toString.call(nul)  //[object Null]
  Object.prototype.toString.call([1])  //[object Array]
  Object.prototype.toString.call({a:1})  //[object Object]
  Object.prototype.toString.call(new Function())  //[object Function]
  ```

  

基本数据类型：Undefined、Null、Boolean、Number、String 

引用数据类型：Object、Array、Function

es6新增数据类型：Symbol,是基本数据类型