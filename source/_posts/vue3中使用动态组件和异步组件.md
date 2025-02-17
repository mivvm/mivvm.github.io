---
layout: vue3中使用动态组件和异步组件.md
title: vue3中使用动态组件和异步组件
date: 2024-06-20 13:18:57
tags: [vue3]
categories: 
- js
- vue3
---

### 起因
最近有个需求是，页面内容由后端返回可以展示哪些模块，以及这些模块的顺序，又到组件很多，所以想到了`动态组件`和`异步组件`的结合使用。
当然后端返回的仅仅是这些模块的名字（即组件的名字）和顺序。
### 前期准备
1.将所有的组件放到`view/template`下面，如`demo1/index.vue`、`demo2/index.vue`这种。
2.要求后端返回的数据为`demo1`、`demo2`等。

正文开始：
### 动态组件
动态组件平常用的比较多，这里就不详细介绍了，有一个点需要注意下：
1.动态组件的`is`可以是字符串，也可以是组件，但是在setup中，也就是`组合式api`中，类型一定要写成组件，如下：

```javascript
<script setup>
import demo1 from "@/views/home/a.vue"
import demo2 from "@/views/home/b.vue"
const nowComponent = shallowRef(demo1)
//这里一定要是组件类型，不能是字符串！
//这里建议用shallowRef，因为用ref在深度监听的情况下，vue会有警告，在这里就不多介绍了
</script>
<template>
	<component :is="nowComponent"></component>
</template>
```
当然如果是`选项式Api`就不用在意了。
### 异步组件
正常使用异步组件的时候，会这样使用：

```javascript
const ComponentName = defineAsyncComponent(() => import(path))
```
在页面中可以用两种使用方法：

```javascript
<template>
	//1.直接使用
	<component-name /> 
	or
	<ComponentName />
	//2.通过component动态组件
	<component :is="ComponentName"></component>
</template>
```
按照我们的需求，根据名字加载组件，然后对`defineAsyncComponent`进行一个封装：

```javascript
function getTemplate(path: string) {
  return defineAsyncComponent(() => import(path))
}
```
紧接着需要查找所有的组件，匹配后端返回的组件进行渲染。
### 读取组件列表

```javascript

import { defineAsyncComponent } from "vue"
//获取所有的组件路径
let modules = import.meta.glob(['@/views/template/**/*.vue'])
interface ITemplate {
  name: string
  path: string
}
let templateArr:ITemplate[] = []
//重构成以组件名字和路径的数组
for(let x in modules) {
  const regex = /src\/views\/template\/(\S*)\/(\S*).vue/; // 要查找的字符串
  let arr = regex.exec(x)!
  if(arr) {
    templateArr.push({
      name: arr[1],
      // path: x 有改动
      path: modules[x]
    })
  }
}
//在上面的基础上对此方法进行改造
function getTemplate(str: string) {
  //根据名字匹配路径
  let obj = templateArr.find(item => str == item.name)!
  //这种写法打包会有问题，import里面的路径不能写成变量，可以写成字符串拼接的格式`../template/${name}/index.vue`
  // return defineAsyncComponent(() => import(obj.path))
  return defineAsyncComponent(obj.path)
}
export default getTemplate
```
到这里各个准备工作已经差不多了，我们准备一个数组作为后端返回的数据：

```javascript
<script setup>
//引入上面封装好的异步组件方法
import getTemplate from "getTemplate.js"
const list = [
	{
		name: 'demo1',
		sortNum: 1,
	},
	{
		name: 'demo2',
		sortNum: 2,
	},
	{
		name: 'demo3',
		sortNum: 3,
	}
]
</script>
<template>
	<div>
		<component v-for="item in list" :key="item.sortNum" :is="getTemplate(item.name)"></component>
	</div>
</template>
```
本以为这样整个功能已经好了，在使用的过程中发现，当页面有元素发生变化的时候，如`v-if`等，就会导致所有的动态组件重新加载，这样肯定不行的。
最后也没找到原因，只是找到了一个解决方法，那就是将`动态组件`那一部分再放到一个组件中，然后主页面中加载这个组件就好了。
本期分享内容就结束了，欢迎大家指导，有更好的方案可以在评论区留言！

