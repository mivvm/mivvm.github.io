---
layout: 浅析css计数器.md
title: 浅析css计数器
date: 2024-03-18 13:06:10
tags: [css]
categories: 
- css
---

计数器有两个关键属性
1. `counter-reset` 计数器重置
	设置初始值：`counter-reset: num 1` 
	默认的初始值为 `0`
	可以设置多个：`counter-reset: num1 1 num2 2;`
2. `counter-increment` 计数器规则，计数器递增的意思
	设置计数：`counter-increment: num 2;`
	默认的值为 `1`
	可以设置多个：`counter-increment: num1 1 num2 2;`
	注：`每次重新设置此属性的时候，counter都会变化`
3. `counter` 计算结果
	使用：`content: counter(num)`
	疑惑：如何能将两个不同的`name`计算

demo1:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .tag {
      counter-increment: num;
    }
    .main {
      counter-reset: num;
    }
    .num::after{
      content: counter(num);
    }
  </style>
</head>
<body>
  <div class="main">
    <div class="tag"></div>
    <div class="tag"></div>
    <div class="tag"></div>
  </div>
  <div class="num"></div>
</body>
</html>
```
demo2:

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    .tag {
      counter-increment: num1 2 num2 3;
    }
    .main {
      counter-reset: num1 num2;
    }
    .num::after{
      content: counter(num1) counter(num2);
    }
  </style>
</head>
<body>
  <div class="main">
    <div class="tag"></div>
    <div class="tag"></div>
    <div class="tag"></div>
  </div>
  <div class="num"></div>
</body>
</html>
```
详细的可以：[参考这个文章](https://segmentfault.com/a/1190000044185526)，写的很详细

