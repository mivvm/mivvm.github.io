---
layout: 
title: 文件url路径转换file对象
date: 2022-04-18 12:21:28
tags: [js, node]
categories: 
- js
- node
---

实现一个小功能，远程url转为file对象
方案一：

```js
function getFileFromUrl(url: string, fileName: string) {
  return new Promise(async (resolve, reject) => {
    const response = await fetch(url);
    const blob = await response.blob()
    let file = new File([blob!], fileName, { type: blob.type });
    resolve(file)
  })
}
```
方案二：

```js
function getFileFromUrl(url: string, fileName: string) {
  return new Promise((resolve, reject) => {
    var blob = null;
    var xhr = new XMLHttpRequest();
    xhr.open("GET", url);
    xhr.responseType = "blob";
    // 加载时处理
    xhr.onload = () => {
      // 获取返回结果
      blob = xhr.response;
      let file = new File([blob!], fileName, { type: blob!.type });
      // 返回结果
      resolve(file);
    };
    xhr.onerror = (e) => {
      reject(e)
    };
    // 发送
    xhr.send();
  });
}
```
使用：

```javascript
getFileFromUrl(url, name).then(res => {
})
```

[参考文章](https://blog.csdn.net/weixin_45577435/article/details/125215196)

