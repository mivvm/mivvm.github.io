---
title: export与export default
date: 2020-03-08 10:59:32
tags: [js, node, es6]
categories: 
- js
- es6
---

es6语法

作用：**都是用于规定模块的对外接口**

使用方法：`import`调用

两者区别：

+ 在一个文件或者模块中，`export`可以有多个，`export default`仅有一个
+ 通过`export`导出的时候要必须用`{}`导出多个模块，`export default`则必须导出一个模块
### export default

```js
//a.js 
var str = 1
export default str
//或者直接抛出
var str = 1
export default {
    a: 1,
    str
}
```

  ```js
//index.js
import index from './a.js'
console.log(index)  //{a: 1, str: 1}
  ```

  ```html
//index.html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>Document</title>
    </head>
    <body>

    </body>
    <script type="module" src="./index.js"></script>
</html>
  ```

### export

```js
//a.js
var str = 1
var m = function() {
    console.log('s')
}
export {str, m}
```

```js
//index.js
import {str} from './a.js'
console.log(str)  //1
//需要哪个就声明哪个
import {str, m} from './a.js'
console.log(str) //1
console.log(m) // ƒ () {console.log('s')}
```

我的理解是`export`是分别导出，使用哪个就导入哪个，而`export default`则是全部导出，导入也是全部导入，在有很多模块下`export`这种方式更好

  注意点：

  1. 因为使用了**模块**`export,import`导出导入，所以html引入script标签的时候要添加`type=module`
  2. 运行环境必须在服务器上，否则会报错。所以可以用`live-server`或者`express`搭建临时环境

  