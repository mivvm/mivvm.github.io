---
layout: 
title: css-grid布局实现一个复杂表格
date: 2024-08-29 13:12:16
tags: [css, grid]
categories: 
- css
- grid
---

产品设计了这样一个表格，如下图：
![在这里插入图片描述](../images/tomcat/pic_17.png)
当然表格内容格式是固定的，本来想用`element ui`的，但是思考了一下，用`el-table`好像嵌套的比较麻烦，还要合并单元格，所以采用了`grid`布局。
废话不多说，直接上代码：

```javascript
<template>
  <div class="table">
    <!-- 表头1 -->
    <div class="th">类型名称</div>
    <div class="th">类型</div>
    <div class="th">标签</div>
    <div class="th">标签编码</div>
    <div class="th">标准定额（元/吨）</div>
    <div class="td td-9">a</div>
    <!-- 一行 -->
    <div class="td td-2">a</div>
    <div class="td">b</div>
    <div class="td">BHGZ</div>
    <div class="td">
      <el-input></el-input>
    </div>
    <!-- 一行 -->
    <div class="td">c</div>
    <div class="td">BHGL</div>
    <div class="td">
      <el-input></el-input>
    </div>
    <!-- 表头2 -->
    <div class="th">
      <div>条件层级</div>
      <div>条件类型</div>
    </div>
    <div class="th">标签</div>
    <div class="th">编码</div>
    <div class="th">浮动金额（±xx/元）</div>
    <!-- 一行 -->
    <div class="td">
      <div>a</div>
      <div>对接</div>
    </div>
    <div class="td">主体对接</div>
    <div class="td">ZTDJ</div>
    <div class="td">
      <el-input></el-input>
    </div>
    <!-- 一行 -->
    <div class="td td-3">
      <div>二级</div>
      <div>重量占比</div>
    </div>
    <div class="td" v-text="'占比≥10%，<20%'"></div>
    <div class="td">LJZB1</div>
    <div class="td">
      <el-input></el-input>
    </div>
    <!-- 一行 -->
    <div class="td" v-text="'占比≥20%，<30%'"></div>
    <div class="td">LJZB2</div>
    <div class="td">
      <el-input></el-input>
    </div>
    <!-- 一行 -->
    <div class="td" v-text="'占比≥30'"></div>
    <div class="td">LJZB3</div>
    <div class="td">
      <el-input></el-input>
    </div>
    <!-- 一行 -->
    <div class="td td-2">
      <div>三级</div>
      <div>张三</div>
    </div>
    <div class="td">mmm</div>
    <div class="td">线</div>
    <div class="td">
      <el-input></el-input>
    </div>
    <!-- 一行 -->
    <div class="td">kkk</div>
    <div class="td">线</div>
    <div class="td">
      <el-input></el-input>
    </div>
  </div>
</template>
<script>
</script>
<style lang="scss" scoped>
.table {
  display: grid;
  grid-template-columns: repeat(5, 1fr);
  border-top: 1px solid #f3f4f5;
  border-left: 1px solid #f3f4f5;
  margin-bottom: 16px;
  font-size: 14px;
  grid-auto-rows: 48px; //额外的行高统一
  .td {
    display: flex;
    align-items: center;
    padding: 0 16px;
    border-bottom: 1px solid #f3f4f5;
    border-right: 1px solid #f3f4f5;
    @for $i from 1 through 15 {
      &-#{$i} {
        grid-row-start: span $i;
      }
    }
    > div {
      flex: 1;
      &:last-child {
        padding-left: 16px;
      }
    }
    // &-2 {
    //   grid-row-start: 2;
    //   grid-row-end: 4;
    //   grid-row-start: span 2;
    // }
  }
  .th {
    display: flex;
    align-items: center;
    background: #eff0f2;
    border-bottom: 1px solid #f3f4f5;
    padding: 0 16px;
    > div {
      flex: 1;
    }
  }
}
</style>
```
就可以实现产品所需的效果了，也庆幸产品的表格是固定的，没有新增删除啥的，要不然真的麻瓜了。。。
上面代码中`td-2`就代表合同两行，已经配置成动态的了，最多可以写到15行。
这个需求也再次认识了`grid`中几个新的api:
1. `grid-auto-rows: 48px; //额外的行高统一`
本来是用的`grid-template-rows: repeat(13, 48px);`但是因为有多个类似表格，行数直接写死的话，不太合适，就发现了这个上面方法，可以替代。
2. `grid-row-start: span 2;`
刚开始合并行是用下面这个代码：

```css
grid-row-start: 2;
grid-row-end: 4;
```
但是这种写法不能通用，发现上面这个方法更好。
[阮易峰老师的grid布局](https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html)
