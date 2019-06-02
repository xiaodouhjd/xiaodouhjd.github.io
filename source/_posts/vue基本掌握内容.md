---
title: vue基本掌握内容
date: 2019-06-02 16:24:18
tags:
      - vue基础
---

### 1-数据监测不到更新操作 和 列表渲染的维护状态
#### 数组：
 ```
    使用一些变异方法或者直接替换数组都能引起页面的更新。

    但是以下两个操作不会：
      1. 直接根据数据下标来修改     vm.list[0] = 9;
      2. 直接修改数据的长度         vm.list.length = 10;

    解决方式：

      1. 使用 Vue.set 原型方法
      2. 使用 vm.$set 实例方法

      Vue.set(target, index, value);
      vm.$set(target, index, value);
```

   #### 对象
```
    Vue 不能检测对象属性的添加或删除：

    Vue.set(target, key, value);
    vm.$set(target, key, value);
```
    
### 列表渲染的维护状态
- 当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。这个类似 Vue 1.x 的 track-by="$index" 。
- 这个默认的模式是高效的，但是只适用于不依赖子组件状态或临时 DOM 状态 (例如：表单输入值) 的列表渲染输出。
- 为了给 Vue 一个提示，以便它能跟踪每个节点的身份，从而重用和重新排序现有元素，你需要为每项提供一个唯一 key 属性：
```
<div v-for="item in items" v-bind:key="item.id">
  <!-- 内容 -->
  </div>
```
  - 建议尽可能在使用 v-for 时提供 key attribute，除非遍历输出的 DOM 内容非常简单，或者是刻意依赖默认行为以获取性能上的提升。
  - 因为它是 Vue 识别节点的一个通用机制，key 并不与 v-for 特别关联，key 还具有其他用途，我们将在后面的指南中看到其他用途。
 -  不要使用对象或数组之类的非原始类型值作为 v-for 的 key。用字符串或数类型的值取而代之。

 ###  v-if 与 v-show 的区别：

      1. 他们都能实现元素的显示隐藏
      2. v-show只是简单控制元素的display属性（不管条件为真还是为假，这个元素都会被渲染出来），而v-if才是条件渲染（条件为真，元素将会被渲染，条件为假，元素会被销毁）
      3. v-show有更高的首次渲染开销。v-if的首次渲染开销要小得多。
      4. v-if有更高的切换开销，v-show切换开销小
      5. v-if有配套的 v-else-if 和 v-else，而v-show没有
      6. v-if可以搭配template使用。而v-show不行。
      
      
### 在 <template> 元素上使用 v-if 条件渲染分组
因为 v-if 是一个指令，所以必须将它添加到一个元素上。但是如果想切换多个元素呢？此时可以把一个 <template> 元素当做不可见的包裹元素，并在上面使用 v-if。最终的渲染结果将不包含 <template> 元素。
**优点是:不影响内部布局。**
```
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p></template>
```
### key 管理复用

<label> 元素仍然会被高效地复用，因为它们没有添加 key 属性。添加key属性就不会复用了。

### 事件修饰符

- .stop
- .prevent
- .capture
- .self.
- once
- .passive
```
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即元素自身触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>
<!-- self修饰符规定的就是，这个事件只能在绑定事件的元素身上起效果。在他的内部元素身上触发事件是不生效的。。他的原理就是判断  target === currentTarget -->

<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>


<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 --> 
<!-- 而不会等待 `onScroll` 完成 --> 
<!-- 这其中包含 `event.preventDefault()` 的情况 --> 
<div v-on:scroll.passive="onScroll">...</div>
```

### 按键修饰符

在监听键盘事件时，我们经常需要检查详细的按键。Vue 允许为 v-on 在监听键盘事件时添加按键修饰符：
```
<!-- 只有在 `key` 是 `Enter` 时调用 `vm.submit()` -->
<input v-on:keyup.enter="submit">
```

你可以直接将 KeyboardEvent.key 暴露的任意有效按键名转换为 kebab-case 来作为修饰符。
```
<input v-on:keyup.page-down="onPageDown">
```

### 按键码
- .enter
- .tab
- .delete (捕获“删除”和“退格”键)
- .esc
- .space
- .up
- .down
- .left
- .right
**注意：有一些按键 (.esc 以及所有的方向键) 在 IE9 中有不同的 key 值, 如果你想支持 IE9，这些内置的别名应该是首选。你还可以通过全局 config.keyCodes 对象自定义**

你还可以通过全局 config.keyCodes 对象自定义按键修饰符别名：
```
// 可以使用 `v-on:keyup.f1`
Vue.config.keyCodes.f1 = 112
```

### 系统修饰键

可以用如下修饰符来实现仅在按下相应按键时才触发鼠标或键盘事件的监听器。
- .ctrl
- .alt
- .shift
- .meta



#### exact 修饰符
修饰符允许你控制由精确的系统修饰符组合触发的事件

##### 鼠标按钮修饰符2.2.0 新增
- .left
- .right
- .middle



###  v-bind:class
 1. 直接使用data的数据，数据是个字符串，这个数据的字符串会直接当做class类名。

 2. 使用 对象 字面量的事项
  key: value, key会作为类名，value会转换成boolean类型，如果为真，这个key就会存在，如果为假，这个key就不存在

 3. 使用 数组的方式
      
### event
事件绑定的时候，事件处理函数如果没有加括号，那么我们的事件处理函数会自动接收到 event 事件对象

如果加上括号，那么 event 对象不会自动传递，如果需要，得手动传递，$event



## 表单绑定
你可以用 v-model 指令在表单 input、textarea 及 select 元素上创建双向数据绑定。它会根据控件类型自动选取正确的方法来更新元素。尽管有些神奇，但 v-model 本质上不过是语法糖。它负责监听用户的输入事件以更新数据，并对一些极端场景进行一些特殊处理。
**注意：v-model 会忽略所有表单元素的 value、checked、selected 特性的初始值而总是将 Vue 实例的数据作为数据来源。你应该通过 JavaScript 在组件的 data 选项中声明初始值。**
v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：
- text 和 textarea 元素使用 value 属性和 input 事件；
- checkbox 和 radio 使用 checked 属性和 change 事件；
- select 字段将 value 作为 prop 并将 change 作为事件。

**对于需要使用输入法 (如中文、日文、韩文等) 的语言，你会发现 v-model 不会在输入法组合文字过程中得到更新。如果你也想处理这个过程，请使用 input 事件。**

### 计算属性
[参考](https://cn.vuejs.org/v2/guide/computed.html)
计算属性，能对现有的data中或者现有的计算属性中的数据做二次处理（计算）。并返回新的数据，这个计算属性可以当做 data 里面的数据一样在页面中或者其他的方法中去使用。
**但是有一点，计算属性不能直接修改**。。。。
他的变化，是基于它的依赖项来的。它的依赖如果发生变化，那么他会重新计算。
计算属性vs方法
 计算属性有缓存，如果依赖项没有变化，那么相同的计算属性我使用多次，只会在第一次的时候执行。后续都是直接拿的缓存。而方法没有缓存
 
```
 computed: { key, value }
        // key - 计算属性的名字，
        // value - 是一个函数 , 他其实就是  amsg 这个计算属性的 getter
```
 
 ### 监听： watch.
监听现有的data或computed中的数据，如果监听的数据有变化，那么监听的回调函数将会被执行。
```
 watch: { key,value}
```
```
        // key - 要监听的数据
        // value - 监听的回调函数 会接收两个参数，newVal, oldVal
        watch: {
        msg (newVal, oldVal) {
          console.log('newVal: ', newVal);
          console.log('oldVal: ', oldVal);
        }
      }
```

### mixin 混入
```
 var common = {}
//定义一个对象，这个对象就是包含有vue组件实例相关的选项的一个对象。
/data如果是一个混入对象的选项，那么data需要使用工厂模式
      // 弄成一个方法，方法里面返回对象
```
```
 // 使用混入
      mixins: [common],
```


### 组件
[参考](https://cn.vuejs.org/v2/guide/components-registration.html)
使用组件
 1. 创建组件
 1. 全局注册  Vue.component(组件的名字，组件的选项对象)
2. 局部注册  components 选项
#### 全局与局部的区别：
1. 在谁的身上注册的局部组件，就只能用在这个人的身上。
2. 全局注册的组件可以用在任意位置。必须要在 new Vue 之前去注册。 
3. 把组件当做一个自定义标签去使用
#### 组件的特性
 这里写法跟 mixin 很像，都是能够使用 new Vue 时传递的哪些选项。
     1. el 与 propsData 不能用
     2. data 必须是一个函数返回对象的形式。
     3. 组件必须要有一个选项是，template 选项，这个选项指定我们组件的内容。
     
     
### 组件的一些使用情况：

  1. 组件的使用，先注册，然后用组件的名字做为标签来使用。
  2. 组件必须要有一个 template 选项，来指定这个组件的模板内容。
  3. 组件的 data 选项 必须是一个函数返回对象的形式。
  4. 组件的命名，可以尽量 短横线方式。
    也可以使用 驼峰的方式，但是使用的时候需要换成 短横线方式。 （todo）
   -  1. 使用 x-template 的 script 标签的方式的时候，不需要考虑这条规则
   -  2. 使用 单文件组件， 也不需要考虑这条规则
  5. 组件的命名不能跟现有的html标签冲突
  6. 组件的 template 选项只能有一个 根元素
  7. 全局注册的组件 Vue.componet 必须写在 new Vue 之前。

**vue 实例也可以设置 template 选项，当有tempalte选项的时候，挂载点会整个被tempalte模板内容给替换。**

#### 局部注册
```
var ComponentA = { /* ... */ }
```

#### template写法
1. 直接使用 模板字符串的方式
  2. 使用单文件组件   （推荐）
  3. 使用 script 标签的方式


### 组件的通信
#### prop

我们可以通过 Prop 向子组件传递数据。用一个形象的比喻来说，父子组件之间的数据传递相当于自上而下的下水管子，只能从上往下流，不能逆流。这也正是 Vue 的设计理念之单向数据流。

#### $emit
 
$emit英式发音：[iˈmɪt]。官方说法是触发当前实例上的事件。附加参数都会传给监听器回调。按照我的理解不知道能不能给大家说明白，先简单看下代码吧：
```
<div id="app">
  <my-button @greet="sayHi"></my-button>
  </div>
  ```
  ```
  复制代码let MyButton = Vue.extend({
  template: '<button @click="triggerClick">click</button>',
  data () {
    return {
      greeting: 'vue.js!'
    }
  },
  methods: {
    triggerClick () {
      this.$emit('greet', this.greeting)
    }
  }
})

new Vue({
  el: '#app',
  components: {
    MyButton
  },
  methods: {
    sayHi (val) {
      alert('Hi, ' + val) // 'Hi, vue.js!'
    }
  }
})
```
复制代码你可以狠狠的戳这里查看Demo! 大致逻辑是酱婶儿的：当我在页面上点击按钮时，触发了组件 MyButton 上的监听事件 greet，并且把参数传给了回调函数 sayHi 。说白了，当我们从子组件 Emit（派发） 一个事件之前，其内部都提前在事件队列中 On（监听）了这个事件及其监听回调。其实相当于下面这种写法：
```
vm.$on('greet', function sayHi (val) {
  console.log('Hi, ' + val)
})
vm.$emit('greet', 'vue.js')
// => "Hi, vue.js"
```


### watch的特殊情况
```
watch: {
//1,全局监听
        msg (newVal, oldVal) {
          console.log('msg 改变了');
        },
//2深入监听
        // userInfo: {
        //   handler (newVal, oldVal) {
        //     console.log('userInfo 改变了');
        //   },

        //   deep: true//深入监听
        // }
//3指定监听
        'userInfo.name': [
          function () {
            console.log('1')
          },

          function () {
            console.log('2')
          }
        ]
      }
```
 
 ### props验证与默认值
 1数组模式
```
  props: ['title'],
```
  2对象模式
```
 props: {
        // key - prop
        // value - 好几种写法，可以对这个prop做一些类型校验。默认值处理之类的。
        // title: String,    // title 字符串类型

        title: {
          type: String,
          default: '关于页'
        },
```





### ref
ref 这个属性，可以写在 普通标签上，也可以写在组件标签上，

1. 普通标签，得到是这个标签的DOM对象
 2. 组件标签，得到是这个组件的组件对象

 ref 用在哪个组件上，那么就需要使用哪个组件的 refs 去获取
 
### 组件的通信的特殊处理
触发事件
```
vm.$emit() // $emit 子 -> 父
```
监听事件
```
vm.$on()
```
监听事件但是只监听一次
```
vm.$once()
```
除了使用 props 来做父->子

 还有一些特殊的操作：（但是不怎么推荐）
    1. ref 用在组件标签上
    2. $root 实例属性，可以拿到根组件的对象，也就是 new Vue 出来的实例
    3. $parent 实例属性，可以拿到当前组件的父组件的实例对象
    4. $children 实例属性，去获取子组件的实例，得到的是一个子组件实例的集合（Array）
    
    
###  兄弟组件之间与复杂关系组件之间的通信
事件总线 - 中央事件管理器 - bus

主要就是使用的 $on 与 $emit

通过一个空的vue实例对象来做为中央事件管理器。



#### 动态组件
```
<component :is="curTab"></component>
```
### 组件上使用v-model
v-model 其实是一种语法糖。
在组件身上使用 v-model 
需要 v-modle 绑定的数据可以在 组件中使用，并且可以通过某种形式直接去修改它。
1. 普通的事件并且是用在普通标签上的时候，$event 代表着事件对象 
2. 如果事件是写在 组件的标签上面，$event 代表着触发这个事件的时候传递过来的参数
### sync修饰符

```
this.$emit('update:title', newTitle)

<text-document v-bind:title.sync="doc.title"></text-document>
```

### 插槽
默认在组件开始标签与结束标签中写的内容是不会理睬的。
如果需要将内容给渲染出去。就需要使用出 插槽(坑) slot


插槽模板内容： 使用组件时开始标签与闭合标签内的内容

使用顺序：
1. 思考 你的 插槽模板内容要显示在组件 template 模板的那个位置。。
 2. 在 组件的 template 模板中 相应的位置 放置一个 slot 内置组件
3. 效果就会是：你这个插槽模板内容会替换 slot 这个组件并渲染到页面上。
 一些情况：
    1. slot 是不限制次数的，
    2. slot 是可以命名的。
 slot 组件上 使用  name 属性给他命名
哪个插槽模板内容要放到哪个 slot 坑里面，就需要带着 name 的名字
 3. slot 还可以有个默认不加锁的 坑
 
 ### 插槽的作用域
 作用域插槽：（把组件内数据能够在插槽模板内容中使用）

 使用步骤：

1. 在 slot 标签上，写 自定义的属性，注意不能是 name2. 在 相应的 slot 的插槽模板内容上面 使用 slot-scope 接收 第一步中传递过来的 porp 集合。



### 插槽的新语法
旧的写法
```
<p slot="abc">我的天</p>
```
新的写法
```
<template v-slot:abc>
<p>我的地</p>
</template>
```
**一个不带 name 的 <slot> 出口会带有隐含的名字“default”**
**v-slot:可以简写成#**
**v-slot 只能添加在一个 <template> 上 (只有一种例外情况)，这一点和已经废弃的 slot 特性不同。**



### 非prop的特性
非props特性: (在使用的组件的时候，在组件标签上写的特性，如果组件内部没有通过props选项去指定，那么这个 特性就叫做 非props特性), 非props特性会自动写入到组件的根元素上
非props特性替换的情况:
合并的情况：（class  yu  style）
禁用非props特性的自动写入到组件根元素上这个规则， 但是 class 与 style 除外。
1. $attrs 与我们 inheritAttrs 没有关系。
2. $attrs 与 inheritAttrs 结合起来使用可以帮我们做一些事情，把这些非props特性写到组件的其他元素上。
基础组件：（将项目常用的一些基础html标签给做一个简单的封装。


### 生命周期图
![图解](ce0b40b24af5aff99b5fc8c085a073ab.bmp)
### Axios
Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

执行 GET 请求// 为给定 ID 的 user 创建请求
```
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// 可选地，上面的请求可以这样做
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
  ```
  ```
  执行 POST 请求axios.post('/user', {
    firstName: 'Fred',
    lastName: 'Flintstone'
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
  ```
可以通过向 axios 传递相关配置来创建请求axios(config)// 
```
发送 POST 请求axios({
method: 'post',
  url: '/user/12345',
  data: {
    firstName: 'Fred',
    lastName: 'Flintstone'
  }});
```

