---
layout: nvm
title: nvm安装node，npm总是安装失败问题解决
date: 2021-07-26 10:35:04
tags: [node]
categories: 
- node
---

nvm安装node，npm安装失败解决方法
<!-- more -->

找到nvm安装目录，打开settings.txt，nvm安装目录一般在
`C:\Users\Admin\AppData\Roaming\nvm`
添加两行代码
```
root: C:\Users\Admin\AppData\Roaming\nvm
path: C:\Program Files\nodejs
node_mirror: https://npm.taobao.org/mirrors/node/
npm_mirror: https://npm.taobao.org/mirrors/npm/
```
参考文章：[nvm安装node，npm安装失败解决方案](https://www.freesion.com/article/5605731454/)
 [nvm下载地址](https://github.com/coreybutler/nvm-windows/releases)