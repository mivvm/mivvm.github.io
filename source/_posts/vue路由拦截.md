---
title: vue路由拦截
date: 2020-04-20 15:53:45
tags: [js, vue]
categories: 
- js
- vue
---

 

### 应用场景

​	需要登录状态的页面

### 实现方式

1. `beforeEach`全局配置

```js
//main.js
router.beforeEach((to, from, next) => {
    let ifInfo = Vue.prototype.$common.getSession('userData');
    if (to.meta.requireAuth) { // 验证是否需要登陆
        if (ifInfo) { // 查询本地存储信息是否已经登陆
      		next();
        } else {
            next({
            	path: '/login', // 未登录则跳转至login页面
            	query: {redirect: to.fullPath} // 登陆成功后回到当前页面，这里传值给login页面，to.fullPath为当前点击的页面
            });
        }
    } else {
    	next();
    }
})
```

```js
//router.js
export default new Router({
  routes: [
   {
      path: '/user',
      name: 'user',
      meta: {
        requireAuth: true // 配置此条，进入页面前判断是否需要登陆
      },
      component: user
    }
  ]
})
```

2. `beforeEnter  `方法也可以实现，不过是局部配置

```js
//router.js
const router = new VueRouter({
    routes: [
        {
            path: '/login',
            name: 'login',
            component: login,
            beforeEnter: (to, from, next) => {
          		console.log('即将进入登录页面')          
          		next()     
            }
        }
    ]
})
```



[参考文章](https://router.vuejs.org/zh/guide/advanced/navigation-guards.html#%E8%B7%AF%E7%94%B1%E7%8B%AC%E4%BA%AB%E7%9A%84%E5%AE%88%E5%8D%AB) 

