---
title: vuex总结
date: 2019-06-15 17:29:09
tags:
      - vuex
---

# vuex
### 什么时候需要使用 vuex

1. 当你不知道该不该用的时候，那就是不该用。（你自会知道什么时候需要用到它）
2. 组件之间通信很复杂的情况下
3. 项目中使用了路由，并且路由页面组件中需要通信的时候，
4. 项目写着写着头发一大把掉的时候。

### vuex的操作流程

1. 先要有仓库，将我们项目中组件上需要共享的数据放置到我们 仓库中 state 的位置。
2. 组件要使用 仓库 中 state 的数据，就从仓库里面拿出来用。
3. 要修改仓库中 state 的数据，
   1. commit(提交) mutation
   2. dispath(派发) action    -> commit mutation
4. 然后仓库中 state 数据发生变化，组件就会得到更新

### vuex 的使用
1. 安装 
```
vuex `npm install --save vuex`
```
2. 项目中 src/store/index.js 文件中创建仓库的实例对象
3. 要 main.js new Vue 的地方配置一个 store 的选项。选项的值就是 2 中 仓库实例对象
   1. 引入 vue
    ```
    import Vue from 'vue'
   ```
   2. 引入 vuex
   ```
   import Vuex from 'vuex'
   ```
   3. 调用 vuex
   ```
   Vue.use(Vuex)
   
   ```
   4. 创建 仓库的实例对象
   ```
   const store = new Vuex.Store({
    state: {
    title: 'hello-world'
   },
   getters: {
   // key - getter 的名字
    // value - 值 函数，要有 return
    // key: value
   },
   mutations: {
    // key - mutation 的名字
    // value - 函数 接收到 state
    // key: value
   },
   actions: {
   }
   })
   ```
   5. 暴露 store
   ```
    export default store;
   ```
   
   
   
 ## State
 ### 对象展开运算符
```
computed: {
    localComputed () {},
  ...mapState({})
}
也可以使用`...mapState([])`,但前提是映射的计算属性的名称与state的子节点名称相同，如果state在vuex的modules中，则不成功。
  ```
### mapState 辅助函数
四种用法
```
computed: mapState({
    // 箭头函数可使代码更简练
    count: state => state.count,

    // 传字符串参数 'count' 等同于 `state => state.count`
    countAlias: 'count',

    // 为了能够使用 `this` 获取局部状态，必须使用常规函数
    countPlusLocalState (state) {
      return state.count + this.localCount
    }
  })
```
```
computed: mapState([
  // 映射 this.count 为 store.state.count
  'count'])
```
### 如何将仓库中的数据拿到组件中去使用

this.$store 就是仓库的实例对象

1. 直接使用 this.$store 的方式在组件 template 模板中使用 （不推荐）
2. 组件中使用 计算属性 去拿 仓库的数据
   ```js
    computed: {
      title () {
        return this.$store.state.title
      }
    }
   ```
3. 借助 vuex 提供的辅助函数 mapState
mapState([])  - 组件的computed 的 key 必须要跟仓库中 state 里面某个key 相同
mapState({})  - 组件的computed 的 key 可以自定义。

4. 将 mapState 与 组件自身的 computed 结合 （推荐）
```js
computed: {
  ...mapState(['title']),
  firstName () {
    return this.name.split('')[0]
  }
},
```
## Getter

mapGetters
mapGetters将store中的getter映射到局部计算属性
```
computed: {
  ...mapGetters([
    'oneGetter',
    'anotherGetter'
  ])，
  ...mapGetters('命名空间的模块名',[ 'oneGetter', 'anotherGetter' ])//第二个获取这个名字的数据

}
```
## Mutation

更改 Vuex 的 store 中的状态的唯一方法是提交 mutation。
每个 mutation 都有一个字符串的 事件类型 (type) 和 一个 回调函数 (handler)。
### mapMutations
使用mapMutations辅助函数将组件中的methods映射为store.commit调用。
```
methods: {
  // 将this.tips映射成 this.$store.commit('tips')
  ...mapMutations(['tips'])
}
```

### 提交载荷（Payload）

你可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）：
```
// ...
mutations: {
  increment (state, payload) {
    state.count += payload.amount
  }}
```
```
//第一个是方法名，第二个是参数
store.commit('increment', {
  amount: 10})
```

提交 mutation 的另一种方式是直接使用包含 type 属性的对象：
```
store.commit({
  type: 'increment',
  amount: 10})
```

**使用常量替代 Mutation 事件类型,
一条重要的原则就是要记住 mutation 必须是同步函数**
### 如何修改仓库中的数据

1. 先需要在仓库中定义我们的mutation
2. 在组件中提交这个mutaion
   1. this.$store.commit('mutaion的名字', 要传递的参数)
   2. this.$store.commit({
     type: mutaion的名字,
     其余的参数
   })
   3. mapMutations 辅助函数
3. mutation不允许异步代码的
## actions

### mapAction
使用mapActions辅助函数将组件的methods映射成store.dispatch调用
```
//第一个是命名空间的名字，模块名，第二个触发的方法名
//当项目只有一个模块时
methods: {
  // 将this.tips映射成 this.$store.dispatch('tips')
  ...mapActions('tips',['tips'])
  ...mapActions(['tips'])
}
```

### 如何写异步代码在仓库中 actions

1. 每一个action里面都可以写异步代码，但是action并不能修改state中的数据，真正修改数据的还是 mutaction
2. 在组件中如何派发这个action
   1. this.$store.dispatch('action的名字')
   2. mapActions 辅助函数
   
   
   
   
   
### vuex 分析图：
![1](1.jpg)

### 10-命名空间
##### 为什么推荐大家在做 vuex moduels 每个模块都加上 命令空间呢

1. 操作方便
  1. mapState(['todos'])     根仓库上拿toods
  2. mapState('todo', ['todos'])   // this.$store.state.todo.todos
  3. mapGetters('todo', ['todos']) // this.$store.getters.todo.todos

#### 如果 不带命名空间会造成什么影响

1. getters 是全局的, 如果在多个模块中有重名的 getter 会报错
2. mutations， actions 有重名的话，会都被执行






最后还有一个看官网
