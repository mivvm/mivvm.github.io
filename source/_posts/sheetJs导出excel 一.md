---
layout: 
title: sheetjs导出excel（一）
date: 2021-11-17 12:01:17
tags: [js, vue, sheetjs]
categories: 
- js
- vue
- sheetjs
---

这里以vue+element为例：
项目遇到这样一个需求，将下列表格导出为excel：

<!-- more -->

![](../images/tomcat/pic_6.webp)

1.安装xlxs依赖
	npm i xlsx

主要的导出excel的方法有`aoa_to_sheet`和`json_to_sheet`两种，这里我主要介绍下后者，主要导出流程如下：

```js
const XLSX = require("xlsx")
var wb = XLSX.utils.book_new()
//arr就是我们所导出的数据
let fdXslxws = XLSX.utils.json_to_sheet(arr)
XLSX.utils.book_append_sheet(wb, fdXslxws, 'sheet1')
XLSX.writeFile(wb, "测试.xlsx")
```


2.`json_to_sheet`的使用，简单来说就是json数据导出excel数据
根据官网所说，最后所需要的数据就是这个格式，即为上文所说的`arr`

```js
[
{
"日期": "2016-05-02",
"姓名": "王小虎",
"地址": "北京"
},
{
"日期": "2016-05-02",
"姓名": "张三",
"地址": "北京"
},
{
"日期": "2016-05-02",
"姓名": "李四",
"地址": "上海"
}
]
```
故，只需要将我们data中的table数据转换为此类格式即可。
完整代码如下:


```js
<script>
const XLSX = require("xlsx")
export default {
    data() {
        return {
            tableData: [{
                date: '2016-05-02',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1518 弄'
            }, {
                date: '2016-05-04',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1517 弄'
            }, {
                date: '2016-05-01',
                name: '王小虎',
                address: '上海市普陀区金沙江路 1519 弄'
            }],
            tableHeader: [
                {
                    title: '日期',
                    map: 'date'
                },
                {
                    title: '姓名',
                    map: 'name'
                },
                {
                    title: '地址',
                    map: 'address'
                },
            ]
        }
    },
    methods: {
        exportExcel() {
            var wb = XLSX.utils.book_new()
            let arr = []
            let newHeaderObj = {}
            this.tableHeader.map(item => {
                newHeaderObj[item.map] = item.title
            })
            this.tableData.map((el, idx) => {
                let obj = {}
                for (let x in newHeaderObj) {
                	 obj[newHeaderObj[x]] = el[x]
                }
                arr.push(obj)
            })
            let fdXslxws = XLSX.utils.json_to_sheet(arr)
            XLSX.utils.book_append_sheet(wb, fdXslxws, 'sheet1')
            XLSX.writeFile(wb, "测试.xlsx")
        }
    }
}
</script>
```
也有按需合并单元格等诸多强大功能，感兴趣的可以去官网看下！
以下附上`xlsx`[依赖地址](https://www.npmjs.com/package/xlsx#array-of-arrays-input)和[中文文档](https://github.com/rockboom/SheetJS-docs-zh-CN/)





