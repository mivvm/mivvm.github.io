---
title: 对象支持for-of方法
date: 2021-01-22 10:53:56
tags: [es6, js]
categories: 
- js
- es6
---



为什么对象不支持`for-of`方法

因为`for-of `循环首先会向被访问对象请求一个迭代器对象，然后通过调用**迭代器**对象的next() 方法来遍历所有返回值。 

打印对象发现

```js
var obj = {
    a: 1
}
console.log(obj[Symbol.iterator])  //undefined
var arr = [1,2,3]
console.log(arr[Symbol.iterator])  //ƒ values() { [native code] }
//发现对象本身或者原型链上不存在Symbol.iterator方法，所以不能一直用for-of遍历
```

人工添加一个`Symbol.iterator`

```js
obj[Symbol.iterator]= function() {
    var keys = Object.keys(this)
    var count = 0
    return {
        next() {
            if(count < keys.length) {
                return {
                    value: obj[keys[count++]],
                    done: false
                }
            }else {
                return {value:undefined,done:true};
            }
        }
    }
}
let o = obj[Symbol.iterator]()
console.log(o.next())  //{value: 1, done: false}
console.log(o.next())  //{value: undefined, done: true}
for(var x of obj) {
    console.log(x)  //1
}
```

