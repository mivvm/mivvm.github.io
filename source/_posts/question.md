---
title: 前端小问题整理
description: 平常项目中碰到的css,js,html以及其他的小问题整理。
tag: css,js,html

---

### rem函数

```javascript
(function() {
    var b = document.documentElement,
        a = function() {
            var w = b.getBoundingClientRect().width;
            b.style.fontSize = (100/750) * (w >= 750 ? 750 : w) + 'px';
        },
        c = null;
    window.addEventListener('resize', function() {
        clearTimeout(c);
        c = setTimeout(a, 300);
    })
})()
```



### 获取url参数集合

```javascript
function GetParams(temp, name) {
		var theRequest = new Object();
		var strs = temp.split("&");
		for (var i = 0; i < strs.length; i++) {
			theRequest[strs[i].split("=")[0]] = (strs[i].split("=")[1]);
		}
		// 获取 参数对象的指定key 的 value值
		var result = theRequest[name] || null;
		return result;
	}
function getUrlParamVal(name) {
    // 获取当前 URL参数集
    var r = decodeURI(window.location.search);
    var arr1 = r.split("?");
    arr1.shift();
    var params = arr1.join("&");
    var res = GetParams(params, name);
    return res;
}
```



### 伪元素清除浮动

```css
.clearfix:after {
    content: '.';
    height: 0;
    display: block;
    clear: both;
    visibility: hidden;
}
.clearfix{
    display: block; 
    zoom: 1;
}
```

### 满屏背景图片在不同分辨率下的自适应

```
.box {
    width: 1920px;
    position: relative;
    left: 50%;
    margin-left: -960px;
}
```

### Map和forEach的区别

```javascript
var arr = [1,2,3,4,5];
arr.forEach((item, index) => {
    return arr[index] = item * 2;
})
console.log(arr)
var arr1 = [1,2,3,4,5];
var arr2 = arr1.map(item => item * 2);
console.log(arr1)
console.log(arr2)
Map会返回新的数组，不会修改原来的数组，而forEach则是在原数组上面修改
```



### 正则之银行卡号

```javascript
function bankCardNumberSerialize(obj){
    var num = 0;
    var str = obj.val();
    //var elem = obj;
    var newValue = ''
    if (str.length > num) {
        var c = str.replace(/\s/g, "");
        if (str != "" && c.length > 4 && c.length % 4 == 1) {
            newValue = str.substring(0, str.length - 1) + " " + str.substring(str.length - 1, str.length);
            obj.val(newValue);
        }
    }
    num = str.length;
}
```

### 小程序md5加密

```
小程序使用md5加密的时候，数字要转换成字符串，中文要转成utf-8，否则与后端的md5加密不一致
```

