---
title: vue脚手架
date: 2019-06-15 17:33:54
tags:
   - vue-cli
---
# vue脚手架
[vue脚手架官网](https://cli.vuejs.org/zh/)
##### 首先安装脚手架:
```
npm install -g @vue/cli
OR
yarn global add @vue/cli
```
##### 这个命令来检查其版本是否正确:
```
vue --version
```

##### 创建一个项目
```
vue create hello-world
```
最后把自己所需的选项选好，命名就创建完成了

##### 使用图形化界面
```
vue ui
```
![b445678f1a79c36cb74b54971e2959f9.png](en-resource://database/799:1)
##### 开发时
```
npm run serve
```

##### 生产上线时
```
npm run build
```

##### 测试
```
npm run test
```

##### 运行 eslint 的检查，并且会自动修复一些 eslint 的错误
```
npm run lint
```
# 项目的目录结构
```
- public      静态资源文件夹，它里面的内容是不需要 webpack 做打包构建的内容。
  - favicon.ico
  - index.html        用于 webpack 主动生成 index.html 的模板
- src         项目源文件夹，你写代码的位置。这个里面的所有内容，都会经过webpack的打包构建
  - assets    静态资源文件夹，这个里面主要放 css img font 之类的东西，
  - components  vue的组件
  - App.vue     老二组件
  - main.js     项目的入口js文件
- .browserslistrc   浏览器的兼容配置文件，它主要是给到 postcss 使用的。
- .editorconfig     告诉我们编辑器的配置
- .eslintrc.js      eslint 的规则配置文件
- .gitignore        git 忽略文件
- babel.config.js   babel 的配置，.babelrc
- package-lock.json npm 锁文件
- package.json      npm 依赖项，与当前项目的描述
- postcss.config.js postcss 的配置文件
- README.md         读我文件
```
### 项目启动

### 假数据 - 

### 1. 自己写json文件

> 假数据应该放到 public 文件夹下

todos.json

### 2. mockjs

> 一款能够创建假数据，并且这个假数据还有些真实的工具。它主要是拦截我们的ajax请求来实现。

1. 安装
```sh
npm install --save mockjs
```
2. 去编写 mockjs 的拦截代码 src/mockdata/index.js
3. 在 main.js 中，引入第2步中的文件

### json-server
> 前端开发，很多时候，我们前端的页面已经做好了，下一步就是要跟后台做接口的联调，但是这个时候后台可能还没有写完。这个时候就需要我们自己来造假数据，好使前端的业务逻辑能够跑起来。
> 使用  json-server 来模拟假数据

todo.json

{
  data: [
    { name: '吃饭', id: 1 },
    { name: '睡觉', id: 2 }
  ]
}

{
  data: [
    { aname: '吃饭', aid: 1 },
    { aname: '睡觉', aid: 2 }
  ]
}
[json-server官网](https://www.npmjs.com)
**入门**
**前端人员开发必备**

##### 安装JSON服务器
```
npm install -g json-server
```

##### 创建db.json包含一些数据的文件
```
{
  "posts": [
    { "id": 1, "title": "json-server", "author": "typicode" }
  ],
  "comments": [
    { "id": 1, "body": "some comment", "postId": 1 }
  ],
  "profile": { "name": "typicode" }
}
```

##### 启动JSON服务器
```
json-server --watch db.json
```

##### 路线
根据以前的db.json文件，这里是所有默认路由。您还可以使用添加其他路线--routes。
**多条路线**
```
GET    /posts
GET    /posts/1
POST   /posts
PUT    /posts/1
PATCH  /posts/1
DELETE /posts/1
```


### 最后封装axios
1. 在 src 下面 新建一个文件  src/utils/http.js
2. 在 http.js 中做封装，并且暴露出去
3. 在 main.js 将 2 中的东西给添加到 vue 的原型上面。后续可以使用 this.$http 来使用

// 1. 引入 axios
```
import axios from 'axios';
```
// 2. 生成一个 axios 的实例
```
const http = axios.create();
```
// 3. 设置 http 的一些默认选项
```
http.defaults.baseURL = process.env.NODE_ENV === 'production' ? 'http://www.baidu.com/' : 'http://localhost:3000/'; // 基准url地址
http.defaults.timeout = 10000; // 超过这个时间点请求还没有完成，那么就报错。
```
// 4. 做请求的拦截

// 5. 做响应的拦截
```
http.interceptors.response.use(response => {
  return response.data;
})
```
// 6. 不要忘了暴露
```
export default http;
```
// http.get()
// http.post()
// http.delete()


