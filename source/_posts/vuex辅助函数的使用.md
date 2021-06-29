---
title: vuex辅助函数的使用
date: 2020-07-18 17:30:06
tags: [vue, vuex]
categories: 
- js
- vue
---

1.  什么是辅助函数

   为组件创建`计算属性`以返回 Vuex store 中的状态 

2. 应用场景

   当一个组件需要获取多个状态的时候，将这些状态都声明为计算属性会有些重复和冗余。为了解决这个问题，我们可以使用 `mapState` 辅助函数帮助我们生成`计算属性 `

3. 辅助函数的分类：

   **mapState**、**mapGetters**、**mapActions**、**mapMutations** 

4. 使用方法：

   ```js
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
               console.log(payload)
               state.count = payload
           }
       },
       actions: {
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

   

   1. `mapstate`：把 state 属性映射到 computed

      ```js
      import { mapState } from 'vuex'
      export default {
          computed: {
              ...mapState(['count']),
              ...mapState({
                  s1: 'num',
                  s2: 'count'
              }),  //防止store里面的变量与当前data里面的变量冲突，可以使用这种方式重命名
          },
          created() {
              console.log(this.s1)
          },
      }
      ```

      

   2. `mapGetters`: 把 getters属性映射到 computed

      ```js
      import { mapGetters } from 'vuex'
      export default {
          computed: {
              // ...mapGetters(['doneTodos'])
              ...mapGetters({
                  doneTodos: 'doneTodos'
              })
          },
          created() {
              console.log(this.doneTodos)
          },
      }
      ```

   3. `mapMutations`: 把mutations里的方法映射到methods中

      ```js
      import { mapMutations } from 'vuex'
      export default {
          methods: {
              ...mapMutations(['increment', 'edit']),
              modify() {
              	this.increment()
              	this.edit(16)
          	}
          }
      }
      ```

   4. `mapActions`: 把mapActions里的方法映射到methods中

      ```js
      import { mapActions } from 'vuex'
      export default {
          methods: {
              ...mapActions(['aEdit']),
              modify() {
              	this.aEdit(99)
          	}
          }
      }
      ```

      