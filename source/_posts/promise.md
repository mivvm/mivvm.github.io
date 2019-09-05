---
title: promise
date: 2019-09-05 16:39:04
tags: js,promise
---



### Then,catch,all

	- 两者都会返回一个新的promise实例
	- `catch`只会监测当前catch前面的`reject`函数，并不会监测后面的`reject`
 - all  
   	- 1）只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数。
   	- 2）只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数。

