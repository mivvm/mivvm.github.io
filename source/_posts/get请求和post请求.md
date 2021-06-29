---
title: get请求和post请求
date: 2020-07-29 09:36:11
tags: [js, get, post]
categories: 
- js
---

`get和post`是`http`请求的两种方式，其不同的如下：

 	1. **应用场景**：`get`类似于读取，应用于不会对服务器资源产生影响的场景，而`post`则应用于对服务器资源产生影响的场景，如用户注册等
		2. **是否缓存**：浏览器一般会对`get`请求缓存，很少对`post`缓存
		3. **安全性**：`get`可以将请求的参数放入`url`中，发送到服务器，是**可见**的，所以最好不要传递敏感信息，而post请求则不会
		4. **请求长度**：由于浏览器对`url`长度有限制，所以影响`get`发送数据时的长度
		5. **参数**： `post `的参数传递支持更多的数据类型 ,而且是放到`send`方法里面的

```js
//get请求
let xhr = new XMLHttpRequest()
xhr.open('get', url+params, true)
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && (xhr.status === 200 || xhr.status === 304)) {
        console.log(xhr.responseText);
    }
}
xhr.send()
```

```js
//post
let xhr = new XMLHttpRequest()
xhr.open('post', url, true)
xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");  
xhr.onreadystatechange = function () {
    if (xhr.readyState === 4 && (xhr.status === 200 || xhr.status === 304)) {
        console.log(xhr.responseText);
    }
}
xhr.send(params)
```

`readyState`的值：

+ 0：请求未初始化（代理被创建，但尚未调用 open() 方法） 
+ 1：服务器连接已建立（`open`方法已经被调用） 
+ 2：请求已接受（`send`方法已经被调用，并且头部和状态已经可获得） 
+ 3：请求处理中（下载中，`responseText` 属性已经包含部分数据） 
+ 4：请求已完成，且响应已就绪（下载操作已完成） 

常见http状态码：

+ 100：客户端应该继续发送请求 
+ 200：成功接受请求
+ 301：重定向
+ 404：请求资源不存在
+ 500：服务器遇到未知错误