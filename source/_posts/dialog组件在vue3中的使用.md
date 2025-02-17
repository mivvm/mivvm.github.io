---
layout: dialog组件在vue3中的使用.md
title: dialog组件在vue3中的使用
date: 2024-01-12 13:01:28
tags: [vue3]
categories: 
- js
- vue
---

项目中通过`pinia`做状态管理，在`dialog`组件中使用，发现了一些问题，在此总结一下。
<!--more-->
+ store

```js
//store.ts
import { defineStore } from "pinia"
const useTestStore = defineStore({
    id: 'testStore',
    state() {
        return {
            obj: {
                name: '张三'
            }
        }   
    },
})
export default useTestStore
```
+ dialog

```js
//dialog
<template>
  <div>
    <el-dialog
      v-model="dialogVisible"
      title="Tips"
      width="500"
      destroy-on-close
    >
      <span>This is a message</span>
      <child />
      <template #footer>
        <div class="dialog-footer">
          <el-button @click="dialogVisible = false">Cancel</el-button>
          <el-button type="primary" @click="cancel"> Confirm </el-button>
        </div>
      </template>
    </el-dialog>
  </div>
</template>

<script>
import child from "./child.vue"
import useTestStore from "../../store"
export default {
  components: {
    child,
  },
  data() {
    return {
      dialogVisible: false,
    }
  },
  computed: {
    ...mapState(useTestStore, ["obj"]),
  },
  methods: {
    open() {
      this.dialogVisible = true
    },
    cancel() {
      this.dialogVisible = false
      this.$emit("update:modelValue", new Date().getTime())
    },
  },
  unmounted() {
	//需要重置store
    useTestStore().$reset()
    console.log("弹窗销毁")
  },
}
</script>
```
+ dialog中的子组件

```js
//child
<template>
  <div>123 {{ obj.name }}</div>
</template>
<script>
import useTestStore from "../../store"
export default {
  computed: {
    ...mapState(useTestStore, ['obj'])
  },
  watch: {
    obj: {
      handler(newValue) {
        console.log('watch', newValue)
      },
      deep: true,
    }
  },
  unmounted() {
    console.log('弹窗中的组件销毁')
  }
}
</script>
```
一些情况下，在使用弹窗的时候，需要在弹窗上加一个`key`，保证弹窗关闭的时候，数据重置更新。代码如下：

```js
//页面中
<template>
  <div> 
    <button @click="cccOpen">打开弹窗</button>
    <ccc ref="cccDom" :key="num2" v-model="num2" />
  </div>
</template>
<script>

import ccc from "./components/ccc/index.vue"
export default {
  components: {

    ccc
  },
  data() {
    return {

      num2: 1
    }
  },
  methods: {
    cccOpen() {
      this.$refs.cccDom.open()
    }
  },
  unmounted() {
    console.log('页面销毁')
  }
}
</script>
```

最后运行发现，每次关闭弹窗，然后打开弹窗，`child`组件的`watch`方法累加,进而导致很多问题。
问题出现原因：

1. `useTestStore().$reset()` （我也没想明白，反正和它有关）
2. `key`
因为弹窗内部还未销毁的时候，弹窗本身已经销毁了，导致后续的销毁未继续执行，所以每次关闭弹窗的时候都会累加。
总而言之，就是因为`store中reset方法`和`key`的双重作用下导致了这个问题。

在保留以上功能的情况下，如何修改呢？
解决方法如下：

```js
cancel() {
      this.dialogVisible = false

      setTimeout(() => {
        this.$emit("update:modelValue", new Date().getTime())
      }, 0)
    },
```



