---
title: exports与module.exports
date: 2019-07-28 14:36:07
tags: [node, js]
categories: 
- node
---

### 环境

exports与module.exports是必须运行在node环境下面的

### 引入方式 

require

```js
let a = require('./a')
```



### 使用方式

+ 初始值

```js
console.log(exports)  //{}
console.log(modulte.exports) //{}
console.log(exports === modulte.exports) //true
```

​		从上面可知，一开始两者都是空对象，并且指向**同一块内存**

+ 变化

  + exports，只是一个变量名

  ```js
  //a.js
  function a() {
      console.log(1)
  }
  exports.a = a
  //b.js
  let a = require('./a')
  a.a()  //1
  console.log(a)  //{ a: [Function: a] }
  ```

  + module.exports

  ```js
  //a.js
  function a() {
      console.log(1)
  }
  module.exports = a
  //b.js
  let a = require('./a')
  a()  //1
  console.log(a)  //[Function: a]
  
  ```

  ```js
  //a.js
  function a() {
      console.log(1)
  }
  module.exports.a = a
  //b.js
  let a = require('./a')
  a.a()  //1
  console.log(a)  //{ a: [Function: a] }
  ```

  

  由于exports是一个空对象，在使用的时候，需要**exports.xxx=xxxx**，相当于给空对象添加一个属性，**require**之后，得到一个对象**{ a: [Function: a] }**，而**module.exports**使用方式可以选择直接赋值**module.exports = a**,也可以使用**module.exports.a = a**,<font color=red>require</font>**引入的对象本质是module.exports**。所以，**exports**不能直接赋值，赋值会改变内存内置,会使**module.exports !== exports**，这会导致内容失效  

