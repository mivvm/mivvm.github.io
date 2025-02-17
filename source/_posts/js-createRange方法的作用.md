---
layout: js-createrange
title: js-createRange方法的作用
date: 2024-07-18 13:20:56
tags: [js]
categories: 
- js
---

### 因
在使用`element ui table`组件的时候，发现了`tooltip`这个属性，发现它只在元素很长的时候才会生效，那它是怎么判断里面文字的长度的？所以去找了一下源码，就发现了一个这样的方法——`createRange`。
在MDN上面也没有详细的介绍。下面我简单介绍一下它的用法：
### 果

```javascript
<style>
   #box {
     width: 200px;
     height: 40px;
     overflow: hidden;
     white-space: nowrap;
   }
</style>
<div id="box">
    <div class="child">豫章故郡，洪都新府2。星分翼轸，地接衡庐3。襟三江而带五湖4，控蛮荆而引瓯越5。物华天宝，龙光射牛斗之墟6；人杰地灵，徐孺下陈蕃之榻7。雄州雾列8，俊采星驰9。台隍枕夷夏之交10，宾主尽东南之美11。都督阎公之雅望，棨戟遥临；宇文新州之懿范，襜帷暂驻12。十旬休假13，胜友如云；千里逢迎14，高朋满座。腾蛟起凤，孟学士之词宗15；紫电青霜，王将军之武库16。家君作宰，路出名区；童子何知，躬逢胜饯</div>
  </div>
  <script>
    const dom = document.getElementById('box')
    const range = document.createRange();
    range.setStart(dom, 0);
    range.setEnd(dom, dom.childNodes.length);
    const rangeWidth = range.getBoundingClientRect().width;
    console.log(rangeWidth)
```
`setStart`和`setEnd`相当于截取`box`内部的`dom`元素。
从页面看，我们如果直接获取`child`的宽度，只能是`200px`，所以要获取文字的宽度就有点麻烦。而借助`createRange`这个方法可以实现。
今天的分享就到这里了，感兴趣的朋友可以私下尝试下。
