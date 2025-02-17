---
layout: 
title: 想开发这样一个npm包，从class生成css
date: 2024-05-15 13:07:46
tags: [node, js, npm]
categories: 
- js
- node
- npm
---

前情：项目中偶尔要加一些简单的样式，也不通用的样式，如宽度为100，间距12，所以想搞一个`npm`包，通过`class`可以直接生成。

需求就是根据特定的类名生成特定的css文件，如：

    
    
    w-100代表宽度为100px
    m-20代表margin为20
    p-20代表padding为20
    ml-20代表margin-left为20

诸如此类的一些尺寸间距设置。

### 监听目录下的vue文件
因为我现在用的是vue项目，所以目前只做了`vue`文件的。。。


```js
const defaultParams = {
  dir: './src'
}
function createAutoCss(params = defaultParams) {
  const watcher = chokidar.watch(params.dir + '/**/*.vue')
  watcher.on('all', (Event, path) => {
    if(Event == 'add' || Event == 'change') {
      getFile(path)
    }
  })
}
```

### 获取文件内容


```js
function getFile(path) {
  fs.readFile(path, (err, data) => {
    if (!data) return
    let str = data.toString('utf-8')
    let styleArr = getStyle(str)
    if (!styleArr.length) {
      return
    }
    writeFile('demo.css', styleArr)
  });
}
```

### 获取class，并且生成相应的css

```js
function getStyle(str) {
  let arr = str.match(/class="(.*)"/g)
  let classArr = []
  arr.map(item => {
    let d = item.replace(/class=/g, "").replace(/"/g, "").replace(/\s+/g, ' ').split(" ");
    classArr.push(...d)
  })
  let styleArr = []
  classArr.forEach(item => {
    if (mtReg.test(item)) {
      let arr = item.split('-')
      let str = arr[0]
      let num = arr[1]
      let a = str.slice(0, 1)
      let b = str.slice(1)
      let style = ''
      if (b) {
        style = `.${item}{${mMap[a]}-${positionMap[b]}: ${num}px}`
      } else {
        style = `.${item}{${mMap[a]}: ${num}px}`
      }
      styleArr.push(style)
    }
  })
  return styleArr
}
```
### 写入文件 

```js
function writeFile(filePath, content) {
  fs.readFile(filePath, (err, data) => {
    if (err) {
      fs.appendFile(filePath, content.join(''), (err) => {
        if (err) throw err;
        console.log('创建成功');
      });
      return
    };
    let str = data.toString('utf-8')
    let newContent = content.filter(item => str.indexOf(item) == -1)
    fs.appendFile(filePath, newContent.join(''), (err) => {
      if (err) throw err;
      console.log('追加成功');
    });
  });
}
```

然后一个简单的根据`class`生成`css`的插件就完成了，想请大家指教一下，谢谢！

````
npm i size-from-class --save-dev
````