---
title: Promise.all与Promise.race
date: 2021-03-19 14:09:53
tags: [es6, js, promise]
categories: 
- js
- es6
- promise
---

### Promise.all

`Promise.all()`方法用于将多个 Promise 实例，包装成一个新的 `Promise `实例。 

参数可以不是数组，但必须具有 `Iterator `接口，且返回的每个成员都是 `Promise `实例 

```js
var p1 = new Promise((reslove, reject)=> {
    reslove(1)
})
var p2 = new Promise((reslove, reject)=> {
    reslove(2)
})
var p3 = new Promise((reslove, reject)=> {
    reslove(3)
})
var p = Promise.all([p1,p2,p3])
```

p的状态由`p1,p2,p3`共同决定，有两种情况：

+ 状态都是`fulfilled `,p的状态才会变成`fulfilled `，`p1,p2,p3`组成一个数组作为返回值

  ```js
  p.then((res)=> {
      console.log(res) //[1, 2, 3]
  }).catch(res=> {
      console.log(res)
  })
  ```

+ 有一个状态被`rejected`,`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数 

  ```js
  var p2 = new Promise((reslove, reject)=> {
      reject(2)
  })
  p.then((res)=> {
      console.log(res) 
  }).catch(res=> {
      console.log(res) //2
  })
  ```



### Promise.race

`Promise.race()`方法也是是将多个 `Promise `实例，包装成一个新的 `Promise `实例。 

```js
var p1 = new Promise((reslove, reject)=> {
    reslove(1)
})
var p2 = new Promise((reslove, reject)=> {
    reslove(2)
})
var p3 = new Promise((reslove, reject)=> {
    reslove(3)
})
var p = Promise.race([p1,p2,p3])
```

只要`p1`、`p2`、`p3`之中有一个实例率先改变状态，`p`的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给`p`的回调函数 。

```js
var p2 = new Promise((reslove, reject)=> {
    reject(2)
})
p.then((res)=> {
    console.log(res) //1
}).catch(res=> {
    console.log(res)
})
//也就是要确定第一个改变状态的返回值
```

[参考文章](https://es6.ruanyifeng.com/#docs/promise#Promise-prototype-finally)