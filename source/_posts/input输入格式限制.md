---
layout: nvm
title: vue2 element ui input输入格式限制 
date: 2021-08-14 10:38:40
tags: [js, vue]
categories: 
- js
- vue
---

工作中总会碰到一些input输入需要做格式限制，特别是一些金额，数字之类。
针对我平常遇到的常见格式，做了一个封装。

<!-- more -->

```js
import Vue from "vue";

/**
 * element-input 输入限制
 *
 */
//v-positiveInteger  正整数
//v-positiveInteger.2  保留两位小数
//v-positiveInteger.4  保留四位小数
Vue.directive("positiveInteger", {
    bind(el, binding, vnode) {
        let dom = findDom(el, "el-input__inner");
        let modifiers = binding.modifiers
        let mantissa = 0
        for (let x in modifiers) {
            mantissa = x
        }
        el.handler = function () {
            if (vnode.inputLocking) {
                return;
            }
            if(mantissa) {
                let reg = new RegExp(`^\\D*(\\d*(?:\.\\d{0,${mantissa}})?).*$`, 'gmi')
                el.childNodes[1].value = el.childNodes[1].value.replace(reg, "$1");
            }else {
                el.childNodes[1].value = el.childNodes[1].value.replace(/\D+/g, "");
            }
			dom.dispatchEvent(new Event("input"));
            
        };
        dom.addEventListener("compositionstart", () => {
            vnode.inputLocking = true;
        });
        dom.addEventListener("compositionend", () => {
            vnode.inputLocking = false;
            dom.dispatchEvent(new Event("input"));
        });
        dom.onfocus = function () {
            if (dom.value == 0) {
                dom.value = "";
            }
        };
        el.addEventListener("keyup", el.handler);
        el.handler1 = function () {
            el.childNodes[1].value = el.childNodes[1].value;
            dom.dispatchEvent(new Event("input"));
        };
        el.childNodes[1].addEventListener("blur", el.handler1);
    },
    unbind(el) {
        el.removeEventListener("keyup", el.handler);
        el.childNodes[1].removeEventListener("blur", el.handler);
    },
});
//递归查找某个元素节点
function findDom(el, className) {
    let dom;
    for (let i = 0; i < el.childNodes.length; i++) {
        if (el.childNodes[i].nodeType == 1) {
            let str = el.childNodes[i].getAttribute("class");
            if (str.indexOf(className) > -1) {
                dom = el.childNodes[i];
                break;
            } else {
                dom = findDom(el.childNodes[i], className);
            }
        }
    }
    return dom;
}

```
[参考文章](https://juejin.cn/post/6844904199289831437)