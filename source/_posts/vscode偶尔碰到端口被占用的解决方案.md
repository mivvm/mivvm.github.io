---
layout: 
title: windows中vscode偶尔碰到端口被占用的解决方案
date: 2024-02-26 13:03:12
tags:
---

![](../images/tomcat/pic_14.webp)
`vscode`启用服务碰到这种情况该怎么解决？
打开`vscode`终端或者命令行，输入
`netstat -nao | findstr 3003`
查找端口占用情况如下：
![](../images/tomcat/pic_15.webp)
找到图片中，第一行的`pid`，即`7092`, 终端再输入：
` taskkill /F /pid 7092 `
执行结果如下，就代表进程被杀死了，端口也不会继续占用了。
![](../images/tomcat/pic_16.webp)

[参考文章](https://blog.csdn.net/huangwfu/article/details/131599182)
