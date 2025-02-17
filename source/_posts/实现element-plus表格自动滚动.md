---
layout: element-plus表格自动滚动.md
title: 实现element-plus表格自动滚动
date: 2021-08-27 10:45:18
tags: [js, vue]
categories: 
- js
- vue
---

项目可视化大屏中需要表格数据自动滚动，项目基于vue3 + element-plus，方法如下：

<!-- more -->

```js

function useElTableScroll(dom, autoScrollFlag) {
  const scrollTop = ref(0)
  //内容高度
  const scrollHeight = ref(0)
  //滚动区域
  const scrollConHeight = ref(0)
  //定时器
  const timer = ref(null)
  //定时器
  const timerout = ref(null)
  var requestAnimationFrame = window.requestAnimationFrame || window.mozRequestAnimationFrame ||
    window.webkitRequestAnimationFrame || window.msRequestAnimationFrame;
  var cancelAnimationFrame = window.cancelAnimationFrame || window.mozCancelAnimationFrame;
  //是否自动滚动
  const autoScroll = () => {
    if (!autoScrollFlag) {
      return false
    }
    timerout.value = setTimeout(() => {
      scrollConHeight.value = dom.value.$refs.bodyWrapper.clientHeight
      scrollHeight.value = dom.value.$refs.tableBody.clientHeight
      scrollStep()
    }, 3000)
  }
  //滚动添加
  function scrollStep() {
    scrollTop.value++
    if (
      scrollTop.value > scrollHeight.value - scrollConHeight.value &&
      scrollHeight.value > 0
    ) {
      timerout.value && clearTimeout(timerout.value)
      setTimeout(() => {
        scrollTop.value = 0
        dom.value.setScrollTop(scrollTop.value)
        autoScroll()
      }, 3000)
    } else {
      timer.value = requestAnimationFrame(scrollStep)
    }
    dom.value.setScrollTop(scrollTop.value)
  }
  //清除滚动
  function clearScroll() {
    timer.value && cancelAnimationFrame(timer.value)
    timerout.value && clearTimeout(timerout.value)
  }
  //鼠标进入
  function cellMouseEnter() {
    clearScroll()
  }
  //鼠标移出
  function cellMouseLeave() {
    scrollTop.value = dom.value.$el.querySelector(
      ".el-scrollbar__wrap"
    ).scrollTop
    autoScroll()
  }
  return {
    autoScroll,
    clearScroll,
    cellMouseEnter,
    cellMouseLeave
  }
}
export default useElTableScroll
```
setup中调用如下：

```js
const table = ref(null)
const { autoScroll, clearScroll, cellMouseEnter, cellMouseLeave } = useElTableScroll(table, props.autoScroll)
onMounted(() => {
  autoScroll()
})
onUnmounted(() => {
  clearScroll()
})
```
`props.autoScroll`是外部控制是否自动滚动
