---
title: vue正向代理
date: 2019-06-15 17:35:27
tags:
    - 正向代理
---
## 16-2正向代理
### 跨域

1. 只会产生在浏览器上。
2. 域名，端口，协议三者有一个不同，就会跨域
3. 如果是后台代码，向别人的网站偷数据，是不会有跨域问题的。

### 正向代理

1.0.0.1  -> www.facebook.com

1.0.0.1  -> 128.28.28.28 -> www.facebook.com

localhost:8080 -> m.maoyan.com    跨域

localhost:8080 -> localhost:8080/后台服务 -> m.maoyan.com

### 1.在文件夹根目录下创建一个vue.comfig.js
##### vue-cli3.0 需要自己去手动添加vue.config.js文件
```
// 配置文件

module.exports = {
  // 选项
  devServer: {
    // port: 9090
    // 正向代理的配置
    // http://localhost:8080/api/find => http://m.maoyan.com/api/find
    // proxy: 'http://i.waimai.meituan.com/'
    proxy: {
      // key - 请求前缀
      // value - 代理选项设置
      // http://localhost:8080/maoyan/api/find => http://m.maoyan.com/api/find
      '/maoyan': {
        target: 'http://m.maoyan.com/',
        pathRewrite: {
          '^/maoyan': ''
        }
      },

      '/meituan': {
        target: 'http://i.waimai.meituan.com/',
        pathRewrite: {
          '^/meituan': ''
        }
      },

      '/mg': {
        target: 'http://movie.miguvideo.com/',
        pathRewrite: {
          '^/mg': ''
        }
      }
    }
  }
}
```

2请求直接改成本地网络就OK了

```
//http://localhost:8080
 axios.get('http://localhost:8080/maoyan/ajax/cinemaList')
        .then(response => {
          let res = response.data
          console.log(res)
        })
```

--------------------- 

### 正向代理的一些问题

1. 只能用在开发阶段
2. 上线阶段，如果前端代码与后台代码是同一个服务，那么不涉及跨域问题，那么就不要正向代理。
           如果前端代码与后台代码不是同一个服务。这个时候是借助nginx的代理配置来代理请求。如果跨域，可以配置 nginx 的反向代理。


### axios - post请求参数问题

> 很多时候，你会发现，我们用 axios 发生 post 请求，后台告诉你，接收不到你传递过来的参数

1. 让后台改
2. 前端自己改
