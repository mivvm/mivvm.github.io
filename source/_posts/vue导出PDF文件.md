---
title: vue导出PDF文件
date: 2021-06-17 10:12:55
tags: [vue]
categories: 
- js
- vue
---

vue中实现页面导出pdf的功能
1. 所需插件 `html2canvas`和`pdfmake`
2. 原理就是将页面截图，然后生成pdf文件，具体代码如下


```js
const main = ref()
async function exportPdf() {
  const canvas = await html2canvas(main.value, {
    allowTaint: true,
    scale: 2,
  })
  const imgBase64 = canvas.toDataURL("image/jpeg")
  const doc = {
    pageSize: { width: canvas.width, height: canvas.height },
    pageMargins: 0,
    content: [
      {
        image: imgBase64,
      },
    ],
  }
  pdfMake.createPdf(doc).download("test.pdf")
}
```
