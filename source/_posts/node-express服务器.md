---
title: node+express服务器
date: 2020-12-15 11:18:33
tags: [node, js, express]
categories: 
- node
- express
---

### 安装配置

​	node, express, request,mysql,body-parser

### 文件配置

​	新建`app.js`,导入各个模块如下：

```js
const express = require('express')
const request = require('request')
const mysql = require('mysql')
const app = express()
//bodyParser  中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据
let bodyParser = require('body-parser')  
app.use(bodyParser.json());
app.use(bodyParser.urlencoded({extended: false}))
//接下来使用连接池链接数据库
const mysql = require('mysql');
const { json } = require('body-parser');
const pool = mysql.createPool({
    host: 'localhost',
    user: 'root',
    password: 'root',
    database: 'name',
    wait_timeout: 10
})
pool.on('connection', function(connection) {
})
pool.on('error', function(connection) {
})
/* 监听端口 */
app.listen(3000, () => {
    console.log('——————————服务已启动——————————');
})
//创建接口
//获取我的书单列表
app.get('/api/getBooklist', (req, res) => {
    async function getList(res) {
        try {
            let sqlStr = `SELECT id,book_list_name,is_share FROM list WHERE user_id='${user_id}' ORDER BY id DESC `
            let data = await query(sqlStr)
            res.json({
                data: {
                    code: 200,
                    data
                }
            })
        }catch(e) {
            res.json({
                data: {
                    code: 100,
                    msg: '服务器问题'
                }
            })
        }
        
    }
    getList(res)
})
```

到此运行`node app.js`一个简单的后端服务就启动了，就可以使用下`localhost:3000/api/getBooklist`访问接口了

