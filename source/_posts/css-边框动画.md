---
layout: 
title: css-边框动画
date: 2023-03-12 12:42:03
tags: [css]
categories: 
- css
---

一个css边框动画，感兴趣的可以看看

<!--more-->
```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    * {
      margin: 0;
      padding: 0;
      list-style: none;
      --transition-duration: 500ms;
    }

    .box {
      width: 500px;
      height: 300px;
      background-color: #000;
      padding: 10px;
      position: relative;
    }

    .box::before {
      position: absolute;
      content: '';
      display: inline-block;
      top: 10px;
      left: 10px;
      width: 0;
      height: 1px;
      background-color: #fff;
      transition: all ease var(--transition-duration);
      transition-delay: 0s;
    }

    .box::after {
      position: absolute;
      content: '';
      display: inline-block;
      bottom: 10px;
      left: 10px;
      width: 0;
      height: 1px;
      background-color: #fff;
      transition: all ease var(--transition-duration);
      transition-delay: calc(var(--transition-duration)/1.25);
    }

    .box div {
      width: 100%;
      height: 100%;
      position: relative;
    }

    .content::before {
      position: absolute;
      content: '';
      display: inline-block;
      bottom: 0;
      left: 0;
      width: 1px;
      height: 0;
      background-color: #fff;
      transition: all ease var(--transition-duration);
      transition-delay: calc(var(--transition-duration)/1.25);
    }

    .content::after {
      position: absolute;
      content: '';
      display: inline-block;
      bottom: 0;
      right: 0;
      width: 1px;
      height: 0;
      background-color: #fff;
      transition: all ease var(--transition-duration);
      transition-delay: 0s;
    }

    .box:hover::after,
    .box:hover::before {
      width: 500px;
    }

    .content:hover::after,
    .content:hover::before {
      height: 100%;
    }

    .box:hover::before,
    .content:hover::after {
      transition-delay: calc(var(--transition-duration)/1.25);
    }

    .box:hover::after,
    .content:hover::before {
      transition-delay: 0s;
    }
  </style>
</head>

<body>
  <div class="box">
    <div class="content">
    </div>
  </div>
</body>

</html>
```

