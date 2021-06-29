---
title: vuex中mutations与actions的区别
date: 2020-06-08 17:19:54
tags: [vue, vuex]
categories: 
- js
- vue
---

mutation中的操作是一系列的同步函数，用于修改state中的变量的的状态。当使用vuex时需要通过commit来提交需要操作的内容。mutation 非常类似于事件：每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。这个回调函数就是实际进行状态更改的地方，并且它会接受 state 作为第一个参数：

```js
const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        increment(state) {
            state.count++
        },
        edit(state, payload) {
            console.log(payload)
            state.count = payload
        }
    }
})
```

当触发一个类型为 add的 mutation 时，需要调用此函数： 

```js
this.$store.commit('increment')
this.$store.commit('edit', 19)
```

而actions的不同点在于：

- Action 可以包含任意异步操作。
- Action 提交的是 mutation，而不是直接变更状态。

```js
const store = new Vuex.Store({
    state: {
        count: 0
    },
    mutations: {
        edit(state, payload) {
            state.count = payload
        }
    },
    actions: {
        aEdit(context, payload) {
            //context: 与store 实例具有相同方法和属性的对象
            return new Promise((resolve) => {
                setTimeout(() => {
                    context.commit('edit', payload)
                    resolve()
                }, 2000)
            })
        }
    }
})
```

```js
this.$store.dispatch('aEdit', 19)
this.$store.dispatch('aEdit', 19).then(()=> {
    console.log(789)
})
```

Action 函数接受一个与 store 实例具有相同方法和属性的 context 对象，因此你可以调用 context.commit 提交一个 mutation，或者通过 context.state 和 context.getters 来获取 state 和 getters。 所以，两者的不同点如下：

- Mutation专注于修改State，理论上是修改State的唯一途径；Action业务代码、异步请求。
- Mutation：必须同步执行；Action：可以异步，但不能直接操作State。
- 在视图更新时，先触发actions，actions再触发mutation
- mutation的参数是state，它包含store中的数据；actions的参数是context，它是 state 的父级，包含 state、getters

[参考文章](https://zhuanlan.zhihu.com/p/100861873?from_voters_page=true) 