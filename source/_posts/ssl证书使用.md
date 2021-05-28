---
title: ssl证书使用
date: 2019-05-27 18:40:20
tags: ssl,域名,服务器，nginx
---

+ 服务器WWW目录下新建SSL文件夹，将申请的证书复制进去
+ 打开nginx配置文件，一下代码复制上去

	server {
	    listen 443 ssl;
	    server_name m.fuguanjia.vip;
	    ssl_certificate ../ssl/_.fuguanjia.vip_2021-04-27_2021-07-26.crt;
	    ssl_certificate_key ../ssl/_.fuguanjia.vip_2021-04-27_2021-07-26.key;
	    location / {
	        proxy_redirect off;
	        proxy_set_header Host $host;
	        proxy_set_header X-Real-IP $remote_addr;
	        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	        proxy_pass http://127.0.0.1:3000/;
	        client_max_body_size 20m;
	    }
	}
ssl_certificate，ssl_certificate_key的值替换成自己服务器SSL文件路径，修改完成，重启nginx

+ 将443端口放行，服务器要添加443端口规则
+ 至此，可以使用https
+ 我的项目当时是<font color=red>小程序</font>，所以小程序后台要添加服务器域名。

