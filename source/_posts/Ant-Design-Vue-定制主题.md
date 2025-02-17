---
layout: ant-design-vue
title: Ant-Design-Vue-定制主题
date: 2022-02-23 12:15:33
tags: [vue3]
categories: 
- vue3
---

#### 方案一：全局引入
在`main.ts`中引入组件库样式
    @import '~ant-design-vue/dist/antd.less'; // 引入官方提供的 less 样式入口文件
    @import 'your-theme-file.less'; // 用于覆盖上面定义的变量

#### 方案二：按需引入
通过`less` 的`modifyVars`属性
vite.config.js

```js
css: {
  preprocessorOptions: {
    less: {
      modifyVars: {
		//放到一个文件中
        hack: 'true; @import "@/styles/theme.less"',
		//直接使用变量
		'primary-color': '#1DA57A',
        'link-color': '#1DA57A',
        'border-radius-base': '2px',
      },
      javascriptEnabled: true
    }
  },
},
```
这种方法要注意的是，组件是通过`unplugin-vue-components`插件来加载的，所以还要配置如下

```js
Components({
  dirs: ["src/components"],
  resolvers: [AntDesignVueResolver({
    sideEffect: true,
    importStyle: 'less'
  })],
}),
```
参考文章：[官网](https://www.antdv.com/docs/vue/customize-theme-cn) [具体参考此文章](https://blog.csdn.net/weixin_43422861/article/details/127636925)
