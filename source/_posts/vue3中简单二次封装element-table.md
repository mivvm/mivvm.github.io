---
layout: 
title: vue3中简单二次封装element-table
date: 2022-08-17 12:30:17
tags: [vue3]
categories: 
- js
- vue3
---

我们平常都会将element-table封装一次，方便复用，减少不必要的代码。参考`ant-design`的table组件用法，试着封装了一下。

1. 第一种直接使用SFC的vue文件

   ```js
   <template>
     <el-table style="width: 100%">
       <template v-for="item in tableHeader" :key="item.dataIndex">
         <el-table-column show-overflow-tooltip v-if="item.slotName || item.render" :prop="item.dataIndex" :label="item.title"
           :sortable="item.sortable" :align="item.align || null" :width="item.width">
           <template #default="scope">
             <div v-if="item.render" >
               {{ item.render({
                 data: scope.row
               }) }}
             </div>
             <slot v-else :name="item.slotName" :data="scope.row"></slot>
           </template>
         </el-table-column>
         <el-table-column show-overflow-tooltip v-else :prop="item.dataIndex" :label="item.title" :sortable="item.sortable"
           :align="item.align || null" :width="item.width" />
       </template>
       <slot />
     </el-table>
   </template>

   <script lang="ts" setup>
   import { VNodeChild } from 'vue';

   interface tableHeaderItem {
     dataIndex: string
     title: string
     slotName?: string
     align?: string
     width?: string
     sortable?: boolean
     render?: VNodeChild
   }
   const props = defineProps<{
     tableData: any,
     tableHeader: tableHeaderItem[]
   }>()

   </script>

   ```

   + `slotName`可以自定义当前列的表格内容以及样式
   + `render`可以自定义当前列的表格内容

   用法如下：

   ```html
   <template> 
     <div class="table">
        <NewElTable :data="tableData" :table-header="tableHeader" size="small">
          <template #name="{ data }">
            <div>{{ data.name }}</div>
          </template>
        </NewElTable>
      </div>
   </template>
   <script setup>
      import NewElTable from "@/components/new-el-table/index.vue"
      const tableHeader = [
        {
          dataIndex: 'date',
          title: '日期',
        },
        {
          dataIndex: 'name',
          title: '名字',
          slotName: 'name',
        },
        {
          dataIndex: 'address',
          title: '地址',
          render({ data }) {
            //在这里可以做一些map解构或者计算
            return data.address
          }
        }
      ]
      const tableData = [
        {
          date: '2016-05-03',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles',
        },
        {
          date: '2016-05-02',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles',
        },
        {
          date: '2016-05-04',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles',
        },
        {
          date: '2016-05-01',
          name: 'Tom',
          address: 'No. 189, Grove St, Los Angeles',
        },
      ]
   </script>
   <style scoped>
     .table {
       width: 500px;
     }
   </style>
   ```

2. 使用`t/jsx`来封装

   ```html



      ```jsx
      import { defineComponent } from "vue";
      import { ElTable, ElTableColumn } from "element-plus";
      export default defineComponent({
        props: {
          tableHeader: {
            type: Array,
            default: []
          }
        },
        setup(props) {
          const { tableHeader } = props
          return () =>
            (
              <ElTable  style="width: 100%">
                {
                  tableHeader.map(item => {
                    if (item.render) {
                      return <>
                        <ElTableColumn show-overflow-tooltip prop={item.dataIndex} label={item.title} sortable={item.sortable} align={item.align || null} width={item.width} 
                          v-slots={{
                            default: (scope) => <>
                              {
                                item.render({
                                  data: scope.row
                                })
                              }
                            </>
                          }}>
                        </ElTableColumn>
                      </>
                    } else if (item.slotName) {
                      return <>
                        <ElTableColumn show-overflow-tooltip prop={item.dataIndex} label={item.title}
                          sortable={item.sortable} align={item.align || null} width={item.width} v-slots={{
                            default: (scope) => <>
                              {this.$slots.default(scope.row)}
                            </>
                          }}>

                        </ElTableColumn>
                      </>
                    } else {
                      return <ElTableColumn show-overflow-tooltip prop={item.dataIndex} label={item.title}
                        sortable={item.sortable} align={item.align || null} width={item.width}>
                      </ElTableColumn>
                    }

                  })
                }
              </ElTable>
            )

        }
      })
   ```

      这种方式和上面的区别主要在于`render`，可以使用`h`函数，也可以使用`t/jsx`抛出的方法，如下：

   ```html
   //demo.jsx
   export default function demo({data}) {
     return (
       <>
       	<div>{data.address}</div>
       </>
     )
   }
   ```

   ```html
   //app.vue

   <template>
     <div class="table">
       <NewElTable :data="tableData" :table-header="tableHeader" size="small">
         <template #name="{ data }">
           <div>{{ data.name }}</div>
         </template>
       </NewElTable>
     </div>
   </template>
   <script setup>
     import NewElTable from "@/components/new-el-table/index.vue"
     const tableHeader = [
       {
         dataIndex: 'date',
         title: '日期',
       },
       {
         dataIndex: 'name',
         title: '名字',
         slotName: 'name',
         render({ data }) {
           return h('div', {
             innerHTML: data.name,
             style: {
               color: 'red'
             }
           })
         }
       },
       {
         dataIndex: 'address',
         title: '地址',
         render: demo
       }

     ]
     const tableData = [
       {
         date: '2016-05-03',
         name: 'Tom',
         address: 'No. 189, Grove St, Los Angeles',
       },
       {
         date: '2016-05-02',
         name: 'Tom',
         address: 'No. 189, Grove St, Los Angeles',
       },
       {
         date: '2016-05-04',
         name: 'Tom',
         address: 'No. 189, Grove St, Los Angeles',
       },
       {
         date: '2016-05-01',
         name: 'Tom',
         address: 'No. 189, Grove St, Los Angeles',
       },
     ]
   </script>
   <style scoped>
     .table {
       width: 500px;
     }
   </style>

   ```

   ​

最后有个疑惑，就是在第一种里面，如何修改可以让`render`支持`h`函数和`t/jsx`