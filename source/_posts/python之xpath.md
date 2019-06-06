---
title: python之xpath
date: 2019-06-04 16:19:08
tags: python,xpath
description: XPath 是一门在 XML 文档中查找信息的语言。XPath 可用来在 XML 文档中对元素和属性进行遍历。XPath 是 W3C XSLT 标准的主要元素，并且 XQuery 和 XPointer 都构建于 XPath 表达之上
---

### 干啥的

 - 可在XML中查找信息 
 - 支持HTML的查找
 - 通过元素和属性进行导航

### 咋调的

```
from lxml import response

selector=response.HTML(源码) #将源码转化为能被XPath匹配的格式

selector.xpath(表达式) #返回为一列表
```

### 咋用的

 - **//** 双斜杠 定位根节点，会对全文进行扫描，在文档中选取所有符合条件的内容，以列表的形式返回
 - **/** 单斜杠 寻找当前标签路径的下一层路径标签或者对当前路标签内容进行操作
 - **/text()** 获取当前路径下的文本内容 
 -  **/@xxxx** 提取当前路径下标签的属性值
 - **|** 可选符 使用|可选取若干个路径 如//p | //div 即在当前路径下选取所有符合条件的p标签和div标签
 - **.** 点 用来选取当前节点 
 - **..**双点 选取当前节点的父节点 