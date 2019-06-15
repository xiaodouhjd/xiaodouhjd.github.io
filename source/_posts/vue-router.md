---
title: vue-router
date: 2019-06-15 17:32:25
tags:
     - vue-router
---
# 前端路由 vue-router
##### 两种应用
- 单页面应用（SPA）single-page-application

- 多页面应用（MPA）一个项目中有多个html文件的。

### vue-router

1. 安装vue-router 
```
 npm install --save vue-router
 ```
2. 项目中选择某个位置放入一个坑 router-view 这个组件
3. 项目中配置路由规则，最后要暴露出去路由器的实例对象
4. 在main.js中也就是 new Vue 的地方需要将第3步中暴露出来的 router 实例对象配置给 根组件 的 router 选项


##### router的使用
0. 引入 vue
```
import Vue from 'vue'
```
1. 引入 vue-router
```
import VueRouter from 'vue-router'
```
1.1 引入路由页面级别组件
```
import Home from '../views/Home.vue'
```
2. 使用 VueRouter 插件
```
Vue.use(VueRouter)
```
3. 配置路由规则 [{}, {}, {}]
```
const routes = [
  {
    path: '/home',
    component: Home,
    children: [
      {
        // /home/page1
        path: 'page1',
        name: 'page1',
        component: Page1
      },
      {
        // /home/page2
        path: 'page2',
        name: 'page2',
        component: Page2
      }
    ]
 ```
4. 实例化路由器对象
```
const router = new VueRouter({
  routes
})
```
5. 将第4步中的东西给暴露出去
### router-view标签(坑)

### router-link标签

> 这也是使用了 路由之后，提供给我们的一个全局组件，它就是一个 a 标签，能帮我们实现路由的切换   
他还有个replace属性

- to 功能与href相似
- active-class  能够让我们修改默认高亮的类的名字 默认值是 'router-link-active'

### router-link 与 a 标签的区别
1. 默认情况下，写不写 # 的问题
2. router-link 可以帮我们实现高亮的效果

### 动态路由匹配

当我们使用上路由之后，会在 Vue 的原型上挂载 两个属性

- $route    当前匹配的路由对象信息
- $router   路由器的实例对象

### 编程式导航 - 通过js代码来控制路由的跳转

$router.push()    跳转页面，新增一个历史记录

$router.back()    后退

$router.forward() 前进

$router.go()      根据参数来看是前进还是后退

$router.replace() 重定向页面，不加历史记录  

### 命令路由

> 在路由规则上给每个规则加上name属性，后续方便我们操作路由的跳转
```
<router-link to="/home/page1"></router-link>
<router-link :to="{ path: '/home/page1', name: 'page1', query: {}, params: {} }"></router-link>
```
to属性传对象的时候，可以有如果属性
- path    路由路径
- name    命名路由的名字
- query   search参数
- params  动态路由匹配参数

**PS: params 与 path 不能共存，如果你要传递 params，那么不要使用 path**


### 命名视图

 (同级) 展示多个视图，而不是嵌套展示，例如创建一个布局，有 sidebar (侧导航) 和 main (主内容) 两个视图，这个时候命名视图就派上用场了。
如果 router-view 没有设置名字，那么默认为 default。
```
<router-view class="view one"></router-view><router-view class="view two" name="a"></router-view><router-view class="view three" name="b"></router-view>
```

一个视图使用一个组件渲染，因此对于同个路由，多个视图就需要多个组件。确保正确使用 components配置 (带上 s)：
```
const router = new VueRouter({
  routes: [
    {
      path: '/',
      components: {
        default: Foo,
        a: Bar,
        b: Baz
      }
    }
  ]})
```

### 重定向和别名
redirect重定向
“重定向”的意思是，当用户访问 /a时，URL 将会被替换成 /b，然后匹配路由为 /b
```
const router = new VueRouter({
  routes: [
    { path: '*', redirect: '/b' }
  ]})
```

重定向的目标也可以是一个命名的路由：
```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: { name: 'foo' }}
  ]})
```

甚至是一个方法，动态返回重定向目标：
```
const router = new VueRouter({
  routes: [
    { path: '/a', redirect: to => {
      // 方法接收 目标路由 作为参数
      // return 重定向的 字符串路径/路径对象
    }}
  ]})
```

##### 别名

“别名”的功能让你可以自由地将 UI 结构映射到任意的 URL，而不是受限于配置的嵌套路由结构。
```
const router = new VueRouter({
  routes: [
    { path: '/a', component: A, alias: '/b' }
  ]})
```


### 路由组件传参

在组件中使用 $route 会使之与其对应路由形成高度耦合，从而使组件只能在某些特定的 URL 上使用，限制了其灵活性。

使用 props 将组件和路由解耦：
**取代与 $route 的耦合**
```
const User = {
  template: '<div>User {{ $route.params.id }}</div>'}const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User }
  ]})
```
**通过 props 解耦**
```
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'}const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]})
```

#### 布尔模式
如果 props 被设置为 true，route.params 将会被设置为组件属性。
#### 对象模式
如果 props 是一个对象，它会被按原样设置为组件属性。当 props 是静态的时候有用。
```
const router = new VueRouter({
  routes: [
    { path: '/promotion/from-newsletter', component: Promotion, props: { newsletterPopup: false } }
  ]})
```
#### 函数模式
你可以创建一个函数返回 props。这样你便可以将参数转换成另一种类型，将静态值与基于路由的值结合等等。
```
const router = new VueRouter({
  routes: [
    { path: '/search', component: SearchUser, props: (route) => ({ query: route.query.q }) }
  ]})
```


### vue路由的两种模式

- hash      (默认)
- history

要修改当前路由模式可以在 实例化 路由器对象的时候设置 mode 选项
```js
new VueRouter({
  mode: 'history'
})
```

#### 这两种模式的区别

1. 从外观上来说，hash模式会在url地址上面有一个 # 号，而 history 没有，更像url地址。
2. 从原理上来说，hash模式是通过 window.onHashChagne 这个事件来处理的。而 history 模式是基于 html5 中 history 新增的一些api. hisotry.pushState() history.replaceState() window.onpopstate 来实现的
3. hisotry模式还需要后台配置去处理上线的404的问题



### nprogress
[更多用法](https://github.com/rstacruz/nprogress)
适用于Ajax'y应用程序的超薄进度条。受Google，YouTube和Medium的启发。

将nprogress.js和nprogress.css添加到项目中。
```
<script src='nprogress.js'></script>
<link rel='stylesheet' href='nprogress.css'/>
```

NProgress可通过bower和npm获得。
```
$ npm install --save nprogress
```

##### 基本用法
只需致电start()并done()控制进度条即可。
```
NProgress.start（）;
NProgress.done();
```


## vue 导航守卫

主要分为三个大块

- 全局
  - 全局前置  beforeEach
  - 全局解析守卫 beforeResolve
  - 全局后置  afterEach
- 路由独享的
  - beforeEnter     进入当前路由时
- 组件级别的
  - beforeRouteEnter    进入当前组件时
  - beforeRouteUpdate   当前组件更新时
  - beforeRouteLeave    退出当前组件时

##### 全局前置守卫
 * 接收一个函数，函数中有三个参数：
 *                  to        -  要去的路由
 *                  from      -  来自哪个路由
 *                  next      -  是一个方法，要不要他去
 * next
 * 1. 如果直接调用，那么就相当于放行
 * 2. 如果调用，但是传递了一个 false, 那么就不放行
 * 3. 如果不调用，也不放行。
 * 4. 调用，并且里面可以传递路由的path路径或者是路由的对象信息。那么就可以重定向到我们的参数中所指定的位置。
 
 ##### 全局后置守卫，
 后置守卫没有 next
### 导航守卫的钩子函数

> 在路由发生变化的时候会主动触发的一些函数

### 作用场景

1. beforeEach 与 afterEach 能实现页面进度条的效果
2. 登录拦截
   1. 先排除 组件级别
   2. afterEach 排除
   3. 正常情况下，如果是后台管理系统的话，因为处理登录与注册页面之外其他的都需要做登录的拦截，那么就可以再 全局前置 里面去做。如果项目中只有那么一个到两个需要做拦截的页面，那么就可以再他们自己的路由独享里面去做。

### 判断是否登录例子
```
router.beforeEach((to, from, next) => {
  NProgress.start()

  // 路由拦截
  if (to.meta.requireLogin) {
    // 判断是否有登录
    if (window.localStorage.getItem('userInfo')) {
      // 有登录
      next()
    } else {
      // 没有登录
      // next('/login')
      console.log(to)
      next({
        path: '/login',
        query: {
          redirect: to.fullPath
        }
      })
    }
  } else {
    next()
  }
})
```

### 路由元信息

定义路由的时候可以配置 meta 字段：
```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      children: [
        {
          path: 'bar',
          component: Bar,
          // a meta field
          meta: { requiresAuth: true }
        }
      ]
    }
  ]})
```
我们称呼 routes 配置中的每个路由对象为 路由记录。路由记录可以是嵌套的，因此，当一个路由匹配成功后，他可能匹配多个路由记录
例如，根据上面的路由配置，/foo/bar 这个 URL 将会匹配父路由记录以及子路由记录。
一个路由匹配到的所有路由记录会暴露为 $route 对象 (还有在导航守卫中的路由对象) 的 $route.matched 数组。因此，我们需要遍历 $route.matched 来检查路由记录中的 meta 字段。
下面例子展示在全局导航守卫中检查元字段：
```
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    // this route requires auth, check if logged in
    // if not, redirect to login page.
    if (!auth.loggedIn()) {
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }})
```