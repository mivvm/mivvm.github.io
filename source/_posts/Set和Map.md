---
title: Set和Map
date: 2019-12-13 14:40:07
tags: js, es6
---

### Set

定义：`Set`本身是一个构造函数，用来生成Set结构数据

#### 特性

  + 成员值唯一，没有重复的值（所以可以用来数组去重，字符串去重）

  ```
  [...new Set(array)]
  [...new Set('ababbc')].join('')
  ```

  

  + 本身是构造函数，所以通过**new**来生成**Set**数据结构
  + 数字NaN不等于NaN，但是在**Set**内部，两个NaN是相等的

  ```js
  let set = new Set()
  let a = NaN
  let b = NaN
  set.add(a)
  set.add(b)
  console.log(set) //Set(1) {NaN}
  ```

  + **Array.from**可以将Set结构转为数组，又提供了一种去除数组重复成员的方法

  ```js
  function dedupe(array) {
    return Array.from(new Set(array));
  }
  dedupe([1, 1, 2, 3]) // [1, 2, 3]
  ```

  + 扩展运算符和Set结构相结合，就可以去除数组的重复成员了

#### 参数

可以接受一个数组，数字，类数组,具有iterable的接口的其他数据作为参数（~~对象不具有iterable接口~~）

#### 操作

+ add方法添加值

  ```js
  const s = new Set()
  s.add(5)  
  console.log(s)  
  //Set(1) {5}
  ```

  

+ delete删除某个值，返回一个布尔值，表示是否删除成功

+ has返回一个布尔值，表示该值是否位**Set**的成员

+ clear清除所有成员，没有返回值

+ size方法获取成员总数

#### 遍历

+ `Set.prototype.keys()`:返回键名的遍历器
+ `Set.prototype.values()`:返回键值的遍历器
+ `Set.prototype.entries()`:返回键值对的遍历器
+ `Set.prototype.foreach()`:使用回调函数遍历每个成员



### Map

定义：由于对象的属性只能接受字符串类型，所以产生了Map结构，优化对象结构

#### 特性

+ key不能重复，重复会覆盖原来的值

+ 只有对同一个对象的引用，Map 结构才将其视为同一个键

  ```js
  const map = new Map()
  map.set({}, 1)
  map.get({})  //undefined
  
  const map = new Map()
  const a = ['a']
  const b = ['a']
  map.set(a, 1)
  map.set(b, 2)
  map.get(a)  //1
  map.get(b)  //2
  ```

  从上面可知Map 的键是跟内存地址绑定的，只要内存地址不一样，就视为两个键，a和b虽然值一样，就是内存地址不一样，这样使用对象作为键名，就不担心同名问题了

+ 键是一个简单类型的数字，字符串，布尔值，只要两个值严格相等，就视为同一个键，特别是`NaN`

#### 参数

+ 接受任何具有iterable接口，且每个成员都是**双**元素的**二维数组**的数据结构
+ map结构和set结构数据都可以作为参数

参考内容：

1. [Set 和 Map 数据结构 - ECMAScript 6入门 ](https://es6.ruanyifeng.com/#docs/set-map)

2. [Set - JavaScript | MDN (mozilla.org](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set))

