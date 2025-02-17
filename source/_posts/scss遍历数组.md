---
layout: scss遍历数组.md
title: scss遍历数组
date: 2021-12-30 12:09:55
tags: [css, scss]
categories: 
- css
- scss 
---

使用scss遍历数组

<!--more -->

### each遍历


```scss
$colors: (
      #00D477,
      #F57933,
      #0052F5
    );

  @each $c in $colors {
    $i: index($colors, $c);

    .tag-#{$i} {
      background-color: $c;
    }
  }
```
生成的结果如下：

```scss
.tag-1 {
  background-color: #00D477;
}

.tag-2 {
  background-color: #F57933;
}

.tag-3 {
  background-color: #0052F5;
}

```
### for循环

```scss
$colors: (
    #00D477,
    #F57933,
    #0052F5
);

@for $i from 1 to 4 {
    .tag-#{$i} {
        background-color: nth($colors, $i)
    }
}
```
生成结果与上方一样，要注意的是`to`循环不到4
另外`to`换成`through`，但是后者可以走到4
