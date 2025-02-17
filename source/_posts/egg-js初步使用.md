---
title: egg.js初步使用
date: 2021-05-28 10:15:34
tags: [node]
categories: 
- node
---


1. 按照[官网](https://eggjs.org/zh-cn/intro/quickstart.html)快速创建一个项目

2. config文件夹作用：环境配置

3. router.js路由

   ```js
   //router.js
   router.get('/index', controller.home.index);
   //home.index指的是home.js里面的index方法，里面是index页面渲染数据
   ```

4. home.js控制器

   ```js
   //controller/home.js
   async index() {
       const { ctx, app, service } = this
       const tplList = app.cache.indexTplList
       const artileList = app.cache.indexArtileList
       const wordList = app.cache.indexWordList
       await ctx.render('index', { tplList, artileList, wordList })
   }
   ```

5. service服务

   服务层里面可以直接连接数据库，查询数据。也可以请求后端接口获取数据

   ```js
   //service/api.js
   async fetchFrontApi(url, data) {
       if (!data) {
           data = {}
       }
       const { frontApiBaseUrl } = this.app.config
       //frontApiBaseUrl根据环境配置在不同文件
       const res = await ctx.curl(frontApiBaseUrl + url, {
           method: 'POST',
           dataType: 'json',
           contentType: 'json',
           data,
           headers
       })
       return res.data
   }
   
   async selectTemplatePage(data) {
       return this.fetchFrontApi('/front2lectTemplatePage', data)
   }
   async selectWordTemplatePage(data) {
       return this.fetchFrontApi('/front2lectWordTemplatePage', data)
   }
   async selectArticlePage(data) {
       return this.fetchFrontApi('/front2lectArticlePage', data)
   }
   
   ```

6. schedule定时任务，缓存处理

   两部分构成，一部分是通过 schedule 属性来设置定时任务的执行间隔等配置

   另一部分是真正定时任务执行时被运行的函数，所以可以用来做一些数据的缓存处理

   ```js
   //schedule/updateCache.js
   const Subscription = require('egg').Subscription;
   
   class UpdateCache extends Subscription {
     // 通过 schedule 属性来设置定时任务的执行间隔等配置
     static get schedule() {
       return {
         interval: '1m', // 1 分钟间隔
         type: 'all', // 指定所有的 worker 都需要执行
       };
     }
   
     // subscribe 是真正定时任务执行时被运行的函数
     async subscribe() {
         const { ctx, service } = this
         const tplList = await service.frontApi.selectTemplatePage({
             isAsc: true,
             pageNum: 1,
             pageSize: 8
         })
         const wordList = await service.frontApi.selectWordTemplatePage({
             pageNum: 1,
             pageSize: 8
         })
         const artileList = await service.frontApi.selectArticlePage({
             isAsc: true,
             pageNum: 1,
             pageSize: 4
         })
         if (!ctx.app.cache) {
             ctx.app.cache = {}
         }
         ctx.app.cache.indexTplList = tplList.data
         ctx.app.cache.indexWordList = wordList.data
         ctx.app.cache.indexArtileList = artileList.data
     }
   }
   module.exports = UpdateCache;
   ```

   这样一个简单的`node+express`项目就可以运行了

[参考文章](https://eggjs.org/zh-cn/basics/structure.html)