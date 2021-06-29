---
title: jq插件扩展
date: 2019-10-06 16:59:15
tags: [js, jq]
categories: 
- js
- jq
---

1. $.extend

   开发静态方法

   ```js
   $.extend({
   	min: function() {
   		console.log(123)
   	}
   })
   $.min()
   ```

   

2. $.fn.extend

   开发成员函数，添加到  JQuery.prototype上面了

   ```js
   $.fn.extend({
       max: function() {
           console.log(456)
       }
   })
   $(window).max()
   ```

   