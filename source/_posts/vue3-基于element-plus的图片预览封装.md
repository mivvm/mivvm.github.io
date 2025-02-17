---
layout: 
title: vue3-基于element-plus的图片预览封装
date: 2024-05-29 13:09:57
tags: [vue3, js]
categories: 
- js
- vue3
---

有时候在表格中需要直接点击一个按钮预览图片，`element-plus`中的图片组件就不方便使用了，所以做了一个简单的封装。
<!--more-->

直接上代码
```js
<template>
  <div v-if="imgList.length">
    <span @click.stop="showViewer = true" v-if="slots.length == 0">预览</span>
    <div v-else @click="showViewer = true">
      <slot :data="imgList"></slot>
    </div>
  </div>
  <teleport to="body">
    <el-image-viewer v-if="showViewer" @close="showViewer = false" :url-list="imgList" :hide-on-click-modal="true" />
  </teleport>
</template>
<script setup>
import { computed } from 'vue';
const slots = Object.values(useSlots())
const props = defineProps({
  imgUrl: ""
})
const imgList = computed(() => {
  if (!props.imgUrl) {
    return []
  }
  if (typeof props.imgUrl == 'string') {
    return props.imgUrl ? props.imgUrl.split(',') : []
  } else {
    return props.imgUrl
  }
})
const showViewer = ref(false)
</script>
<style lang="scss" scoped>
span {
  cursor: pointer;
  color: #0052f5
}
</style>
```
### 使用方式

+ 引入一：

  ```js
  <preview-img :imgUrl="[url,url,url]"></preview-img>
  ```

+ 引入二：

  ```js
  <preview-img :imgUrl="url,url,url">
    <template #default="{data}">//data就是传入的imgList，其实没必要传递出来的，外部本身就可以获取
    <div>预览</div>
    </template>
  </preview-img>
  ```

  ​

`imgUrl`类型可以是数组，英文逗号链接的字符串两种类型

