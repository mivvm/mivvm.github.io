---
layout: js导出文件.md
title: js导出文件
date: 2023-05-09 12:45:43
tags: [js]
categories: 
- js
---

总结了本人工作中常用的三种导出文件的方式

<!--more-->
+ 后端返回的单个链接下载

```javascript
function download(url, filename) {
  let link = document.createElement("a")
  link.style.display = "none"
  link.href = `${url}`
  link.setAttribute("download", `${filename}`)
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
  link = null
}
```
+ 后端返回多个链接，批量下载

```javascript
function batchDownload(data) {
  data.forEach(item => {
    download(item.filePath, item.fileName)
  })
}
```
+ 后端返回一个`流文件`

```javascript
function downloadFileStream(name = "1", content, suffix = "doc") {
  //文件流文件下载
  const blob = new Blob([content])
  const fileName = `${name}.${suffix}`
  const elink = document.createElement("a")
  elink.download = fileName
  elink.style.display = "none"
  elink.href = URL.createObjectURL(blob)
  document.body.appendChild(elink)
  elink.click()
  URL.revokeObjectURL(elink.href) // 释放URL 对象
  document.body.removeChild(elink)
}
```

