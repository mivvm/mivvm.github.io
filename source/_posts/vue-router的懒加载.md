---
title: vue-router的懒加载
date: 2020-03-18 14:29:11
tags: [js, vue]
categories: 
- js
- vue
---

​	

1. 什么是懒加载

   也叫延迟加载，即在需要的时候进行加载，随用随载 

2. 为什么要懒加载

   像vue这种单页面应用，如果没有应用懒加载，运用`webpack`打包后的文件将会异常的大，运行后用户查看相关模块显示的内容时 ，会将整个打包的文件引入而后在其中查找对应的模块然后才将其呈现给用户。需要加载的内容过多，时间过长，会出啊先长时间的白屏，即使做了`loading`也是不利于用户体验，而路由懒加载是将各个模块分开打包，在用户查看下相关模块内容时就直接引入相关模块的打包文件然后进行显示，从而有效的解决了浏览器可能出现短暂时间空白页的情况。 

3. 实现方案

   1. 非懒加载

      ```vue
      import List from '@/components/list.vue'
      const router = new VueRouter({
        routes: [
          { path: '/list', component: List }
        ]
      })
      ```

   2. 使用箭头函数+import动态加载，返回的是路由组件的函数，只有执行此函数才会加载对应的组件，在执行入口main.js时，执行到`List = () => import('@/components/list.vue')`,并没有请求，仅仅是申明了这个路由组件，只有当你点击相应路由链接时，才会请求他，并且只有第一次点击才会请求，后续不会再重复请求

      ```vue 
      const List = () => import('@/components/list.vue')
      const router = new VueRouter({
        routes: [
          { path: '/list', component: List }
        ]
      })
      ```

   3. 使用箭头函数+require动态加载

      ```vue
      const router = new Router({
        routes: [
         {
           path: '/list',
           component: resolve => require(['@/components/list'], resolve)
         }
        ]
      })
      ```

   4. 使用webpack的require.ensure技术，也可以实现按需加载。 这种情况下，多个路由指定相同的chunkName，会合并打包成一个js文件 

      ```vue
      const List = resolve => require.ensure([], () => resolve(require('@/components/list')), 'list');
      const router = new Router({
        routes: [
            {
              path: '/list',
              component: List,
              name: 'list'
            }
      	]
      }))
      ```

      

[参考文章](https://segmentfault.com/a/1190000011519350)