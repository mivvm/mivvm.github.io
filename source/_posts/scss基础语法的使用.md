---
layout: scss基础语法的使用.md
title: scss基础语法的使用
date: 2021-10-12 11:57:18
tags: [css, scss]
categories: 
- css
- scss
---

对scss语法做一个简单的总结。

<!-- more -->

#### 1. 变量声明及使用

```scss
$color: red;
.box {
 	color: $color;
}
```
#### 2. mixin和include
##### 2.1 简单使用


```scss
$bgColor: red;
@mixin bg {
    background: $bgColor;
}
.box {
    @include bg;
}
```
编译结果：

```
.box {
  background: red;
}

```
##### 2.2 进阶使用

```scss
$namespace: 'zz';
@mixin B($name){
    $aaa: $namespace + '_' + $name;
    .#{$aaa} {
        @content;
    }
}
@include B(input) {
    font-size: 12px;
}
```
编译结果：

```css
.zz_input {
  font-size: 12px;
}

```
#### 3. 列表和循环
##### 3.1 for循环

```scss
$list: red, green, blue;
@for $i from 1 through 3 {
    .status-#{$i} {
        color: nth($list, $i)
    }
}
@for $i from 1 to 4 {
    .status-#{$i} {
        color: nth($list, $i)
    }
}
```
注：`nth`可以获取数组的某个元素；
`through`和`to`的编译结果一致，要注意是的`to走不到最后一个数`：

```css
.status-1 {
  color: red;
}

.status-2 {
  color: green;
}

.status-3 {
  color: blue;
}

```
##### 3.2 each循环

```scss
$list:red,green,blur;

@each $item in $list {
    .#{$item} {
        color: $item
    }
}
```
编译结果：

```css
.red {
  color: red;
}

.green {
  color: green;
}

.blur {
  color: blur;
}

```
#### 4. map结构

```scss
$colorMap: (
	fail: 'red',
	success: 'green',
	pendding: 'blue'
);
$bgColorMap: (
	fail: 'red',
	success: 'green',
	pendding: 'blue'
);
@each $key, $value in $colorMap {
    .#{$key} {
        color: $value;
        border-color: map-get($bgColorMap, $key)
    }
}
```
编译结果：

```
.fail {
  color: "red";
  border-color: "red";
}

.success {
  color: "green";
  border-color: "green";
}

.pendding {
  color: "blue";
  border-color: "blue";
}

```

#### 5. 函数

```scss
@function vw($n) {
	@return calc($n*100vw/750)
};
@function px($n) {
	@return calc(#{var(--rootWidth)} * #{$n} / 1920);
}
@function vh($n) {
	@return calc($n * 100vh / 1080);
}
body {
	font-size: vw(10)
}
```
