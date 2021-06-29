---
title: vuex简单使用
date: 2020-05-20 17:01:53
tags: [vue, js, vuex]
categories: 
- js
- vue
---

1. 为什么要使用vuex

   由于传参的方法对于多层嵌套的组件将会非常繁琐，并且对于兄弟组件间的状态传递无能为力。我们经常会采用父子组件直接引用或者通过事件来变更和同步状态的多份拷贝。以上的这些模式非常脆弱，通常会导致代码无法维护。 

   **把应用的所有组件的状态抽取出来，以一个全局单例模式在应用外部采用集中式存储管理**。任何组件都能获取状态或者触发行为 

2. vuex的属性

   + state => 基本数据(数据源存放地)
   + getters => 从基本数据派生出来的数据
   + mutations => 提交更改数据的方法，**同步**，是Vuex修改state的**唯一**推荐方法 
     + 有两个参数一个是`state`，另一个是传参
     + `commit`: 状态改变提交操作方法。对`mutation`进行提交，是**唯一**能执行`mutation`的方法 
   + actions => 像一个装饰器，包裹`mutations`，使之可以异步。
     + 有两个参数一个是`context`，另一个是传参
     + `dispatch`∶操作行为触发方法，是唯一能执行`action`的方法。
   + modules => 模块化Vuex

3. 实例

   ```js
   //store.js
   import Vue from 'vue'
   import Vuex from 'vuex'
   Vue.use(Vuex)
   const store = new Vuex.Store({
       state: {
           count: 0,
           num: 48
       },
       getters: {
           doneTodos: state => {
               return state.num
           }
       },
       mutations: {
           increment(state) {
               state.count++
           },
           edit(state, payload) {
               state.count = payload
           }
       },
       actions: {
           aEdit() {
               setTimeout(() => {
                   context.commit('edit', payload)
                   resolve()
               }, 2000)
           },
           aEdit(context, payload) {
               return new Promise((resolve) => {
                   setTimeout(() => {
                       context.commit('edit', payload)
                       resolve()
                   }, 2000)
               })
           }
       }
   })
   export default store
   ```

   ```vue
   <template>
     <div class="hello">
       <div v-color="style" class="box">{{num}}</div>
       <button @click="modify">修改</button>
       <div v-for="(item, index) in arr" :key="index">{{item}}</div>
     </div>
   </template>
   
   <script>
   import { mapGetters } from 'vuex'
   export default {
     name: 'HelloWorld',
     data() {
       return {
       }
     },
     computed: {
       ...mapGetters(['doneTodos']),
       ...mapGetters({
         doneTodos: 'doneTodos'
       })
     },
     created() {
       console.log(this.doneTodos)
       // console.log(this.$store.getters.doneTodos)
     },
     methods: {
       modify() {
         // this.$store.commit('edit', 16)
         this.$store.dispatch('aEdit', 19).then(()=> {
           console.log(789)
         })
       }
     }
   }
   </script>
   
   ```
   

   

   

   

   

   

   

   

   

