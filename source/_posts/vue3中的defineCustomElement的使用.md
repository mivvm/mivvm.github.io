---
layout: 
title: vue3中的defineCustomElement的使用
date: 2022-07-19 12:28:34
tags: [vue3]
categories: 
- js
- vue
---

偶然发现vue的文档中的有这样一个api：`defineCustomElement`
尝试了很久，才明白用法，在这里分享一下：
#### 1.修改配置
`vite.config.js`

```js
plugins: [
    vue({
      template: {
        compilerOptions: {
          // 将所有带短横线的标签名都视为自定义元素
          isCustomElement: (tag) => tag.includes('-')
        }
      }
    }),
    vueJsx()
  ],
resolve: {
    alias: {
      'vue': 'vue/dist/vue.esm-bundler.js'
    }
}
```
这里是`vite`的配置，其他方式参考[官网](https://cn.vuejs.org/guide/extras/web-components.html#sfc-as-custom-element)
#### 2.创建自定义元素
+ 方式一
	新建`list/index.js`
	
```
import { defineCustomElement } from 'vue'

const MyVueElement = defineCustomElement({
  // 这里是同平常一样的 Vue 组件选项
  props: {
    title: String
  },
  emits: {},
  template: `<div>{{title}}</div>`,

  // defineCustomElement 特有的：注入进 shadow root 的 CSS，行内样式
  styles: [`
    div {
      color: red
    }
  `]
})

// 注册自定义元素
// 注册之后，所有此页面中的 `<my-vue-element>` 标签
// 都会被升级
customElements.define('my-vue-element', MyVueElement)

export default MyVueElement
```
+ 方式二
	新建`list/ui.ce.vue`,文件内容如下：
	
```js
<template>
  <div>测试 {{ title }}</div>
</template>
<script setup lang="ts">
defineProps({
  title: String
})
</script>
```
	在`list/index.js`中引入：

```js
import { defineCustomElement } from 'vue'
import UI from "./ui.ce.vue"
const MyVueElement = defineCustomElement(UI)

// 注册自定义元素
// 注册之后，所有此页面中的 `<my-vue-element>` 标签
// 都会被升级
customElements.define('my-vue-element', MyVueElement)

export default MyVueElement
```
#### 注册组件
`main.js`

```js
import MyVueElement from "./views/defineCustomElement"

```

#### 使用
+ 直接在`vue`或者`html`文件中使用

```
<my-vue-element title="方式一"></my-vue-element>
```
+ 在`js`中

```js
document.body.appendChild(
  new MyVueElement({
    // 初始化 props（可选）
    title: '方式三'
  })
)
```

虽然大概了解了这个api的用法，还有些疑惑
1. `app.config.compilerOptions.isCustomElement = (tag) => tag.includes('-')`这个配置在什么场景生效
2. 	使用场景有哪些
[参考文章](https://www.jb51.net/article/266215.htm#_label1)