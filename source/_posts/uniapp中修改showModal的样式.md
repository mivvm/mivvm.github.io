---
layout: 
title: uniapp中修改showModal的样式
date: 2022-12-17 12:37:38
tags: [uniapp]
categories: 
- uniapp
---


使用uniapp的时候，碰到`showModal`的样式和设计搞一样，在此记录一下。
在`uni.scss`中直接修改你的样式即可：

```css
uni-modal .uni-modal {
	width: 538.46rpx;
	border-radius: 15.38rpx;
	.uni-modal__hd {
		font-size: 32.69rpx;
		font-weight: 500;
		padding: 38.46rpx 38.46rpx 23.08rpx;
		color: rgba(23, 26, 29, 1);
	}
	.uni-modal__bd {
		padding: 0 38.46rpx 30.77rpx;
		color: rgba(23, 26, 29, 0.6);
		line-height: 38.46rpx;
		font-size: 26.92rpx;
	}
	.uni-modal__btn {
		font-size: 32.69rpx;
		font-weight: 500;
		&::after {
			border-color: rgba(236, 236, 236, 1);
		}
	}
	.uni-modal__ft:after {
		border-color: rgba(236, 236, 236, 1);
	}
}
```
如果没有`uni.scss`可在`App.vue`中修改。
首先这种方式只能全局修改，目前没有发现局部修改的方法，有小伙伴有其他的方法的话，可以帮我补充一下，谢谢~~~