---
title: vue反向代理
date: 2019-06-15 17:36:39
tags: 
    - vue反向代理
---

### 反向代理

A - C - B

前端代码的访问地址 - http://118.89.203.190/maizuo
后端接口的方位地址 - http://118.89.203.190:8080

同一台服务器 使用反向代理（nginx) 

http://118.89.203.190/maizuo -> http://118.89.203.190 -> http://118.89.203.190:8080


### 目前我们的vue项目的情况

http://118.89.203.190/maizuo -> 
http://118.89.203.190/mg/lovev/miguMovie/data/seeFilmData.jsp -> 
D http://118.89.203.190:9090/mg/lovev/miguMovie/data/seeFilmData.jsp -> 
http://movie.miguvideo.com/lovev/miguMovie/data/seeFilmData.jsp

? C - B 需要使用nginx反向代理，但是 c - B是不同的两个服务器的服务，nginx是没办法直接去代理的, 它只能是，把同一台服务器上的 80的服务代理到其他的端口服务 （80 -> 9090）


这时，我们需要在自己的服务器上启动一个 9090 的nodejs服务

1. 安装 nodejs 
2. 启动一个后台的服务，服务监听的端口是 9090 （express）
	2.0 在 你自己的电脑上吧 express 项目写好
    创建一个server.js
```server.js
const express = require('express');
const proxy = require('http-proxy-middleware');
const app = express();

app.get('/', (req, res) => {
  res.send('hello world, 我是一个nodejs服务');
})

// http://118.89.203.190:9090/mg/lovev/miguMovie/data/seeFilmData.jsp -> http://movie.miguvideo.com/lovev/miguMovie/data/seeFilmData.jsp
app.use('/mg', proxy({
  target: 'http://movie.miguvideo.com',
  changeOrigin: true, // 主动修改 请求头的 origin ，以防止有些服务对它的限制
  pathRewrite: {
    '^/mg': ''
  }
}))

app.listen(9090)
```
3. express 这个项目，要将 9090 的地址转发到真实的服务器上，需要使用 http-proxy-middleware 这个模块
	3.1 npm install --save http-proxy-middleware
	3.2 对 9090 上某个请求做转发
4. express 这个项目代理配置成功之后，将 node_models 给删除掉再上传到服务器的 /root 目录下
5. 到 服务的 这个express 项目 npm install 。然后启动它
	5.1 为了让 这个后台项目长久运行正常，我们需要守护他的进程
	5.2 npm install -g pm2
6. 接下来，完成 80 -> 9090 服务的反向代理处理
	6.1 找到 nginx 配置文件所在的路径  /etc/nginx/conf.d/default.conf
	6.2 先不要着急去修改这个文件，可以先备份一下。(确保，你已经在 /etc/nginx/conf.d)
		cp default.conf default.conf.bak
	6.3 去修改 default.conf (推荐吧这个文件传到本地电脑上来改，改完之后再上传回去)
	6.4 测试 nginx 配置文件是否有明确的问题  `nginx -t`
	6.5 重启 nginx   `nginx -s reload`
	6.6 测试
