---
title: Fiddler前端妙用
date: 2019-05-13 19:50:46
tags: [js, 抓包, 前端, 调试, fiddler]
categories: 
- js
---

### Fiddler除了抓包还能这样用？---前端使用fiddler线上本地联调

有一些特殊情况，本地环境不能支持完成自测，需要用的线上环境，通过fiddler就可以线上调试自己本地代码并且不影响他人

#### 做法

+ 安装fiddler,打开,右侧配置如下图：
+ ![](Fiddler前端妙用/1.png)

+ 上述勾选好之后,点击  Add Rule,

+ ![](Fiddler前端妙用/2.png)

  注意：<font color=red>本地路径一定要是绝对路径</font>

  然后打开线上地址，就可以调试本地代码了