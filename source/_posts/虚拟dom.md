---
title: 虚拟dom
date: 2020-01-25 10:54:37
tags: [js, vue, dom]
categories: 
- js
- vue
---

1. 什么是虚拟dom

   可以看作是一个使用javascript模拟了DOM结构的树形结构

   ```html
   <div class="box" id="box" style="width: 100px;height: 100px;" onclick="console.log('vdom')">
       <span>vdom</span>
   </div>
   ```

2. 如何解析这个虚拟 dom

   ```js
   const vdom = {
       nodeName: 'div',
       attrs: {
           className: 'box',
           id: 'box' 
       },
       css: {
           width: '100px',
           height: '100px'
       },
       events: {
           onclick: (e)=> {
               console.log(e)
           }
       },
       childrens: [
           {
               nodeName: 'span',
               attrs: {
                   innerText: 'vdom'
               }
           }
       ]
   }
   function render(vdom) {
       const dom = document.createElement(vdom.nodeName)
       const {attrs, css, events, childrens} = vdom
       //添加属性
       for(let item in attrs) {
           dom[item] = attrs[item]
       }
       //样式
       for(let item in css) {
           dom['style'][item] = css[item]
       }
       //添加事件
       for(let item in events) {
           dom[item] = events[item]
       }
       if(childrens && childrens.length > 0) {
           for(let children of childrens) {
               const childrenNode = render(children)
               dom.append(childrenNode)
           }
       }
       return dom
   }
   const dom = render(vdom)
   document.body.append(dom)
   ```

   

 

   

