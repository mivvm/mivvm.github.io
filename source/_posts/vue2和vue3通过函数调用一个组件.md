---
layout: vue2和vue3通过函数调用一个组件.md
title: vue2和vue3通过函数调用一个组件
date: 2022-01-17 12:12:43
tags: [vue2, vue3]
categories: 
- vue2
- vue3
---

实现一个简单的弹窗组件： 
1. vue2 函数式组件写法，通过`extend`方法：
创建`ui.vue`文件，存放我们的组件，如下：

<!--more -->

```js
<template>
    <div class="dialog">
        <div class="dialog-body">
            <div class="dialog-content">
                你好
            </div>
            <div class="dialog-footer" @click="close">关闭</div>
        </div>
    </div>
</template>
<script></script>
<style lang="scss" scoped>
    .dialog {
        width: 100vw;
        height: 100vh;
        position: fixed;
        top: 0;
        left: 0;
        background-color: rgba($color: #000000, $alpha: 0.2);
        &-body {
            width: 500px;
            height: 300px;
            position: absolute;
            left: 50%;
            top: 50%;
            transform: translate(-50%, -50%);
            background-color: #fff;
            border-radius: 4px;
            padding: 10px;
        }
        &-content {
            height: 250px;
        }   
        &-footer {
            height: 50px;
        }
    }
</style>
```
同级目录下创建一个index.js，内容如下;

```js
import UI from "./ui.vue"
import Vue from "vue"
function openDialog() {
    let install = null
    return new Promise((resolve, reject) => {
        const UIconstructor = Vue.extend(UI)
        install = new UIconstructor({
            methods: {
                close(e) {
                    document.body.removeChild(install.$el)
                    resolve(e)
                },
            },
        }).$mount();
        document.body.appendChild(install.$el)
    })
}
export default openDialog
```
在其他页面直接引入调用`openDialog`即可
2. vue3中实现，ui.vue不变，index.js修改如下：


```js
import UI from "./ui.vue"
import { createApp } from "vue"

export default function openDialog() {
  // 创建一个节点，并将组件挂载上去
  const mountNode = document.createElement("div")
  document.body.appendChild(mountNode)
  const app: any = createApp(UI, {
    close: () => {
      app.unmount()
      document.body.removeChild(mountNode)
    },
  })
  app.mount(mountNode)
}
```

