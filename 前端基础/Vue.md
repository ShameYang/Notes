# 一、初识 Vue

## 1.1 简介

1. Vue 是什么？

   一套用于构建用户界面的渐进式 JavaScript 框架

2. Vue 的特点

   - 组件化模式，提高代码复用率、且代码更好维护

   - 声明式编码，编码时无需直接操作 DOM，提高开发效率
   - 虚拟 DOM + Diff 算法，尽量复用 DOM 节点



## 1.2 开发环境搭建

第一步：安装开发版本 vue.js 和生产版本 vue.min.js

第二步：浏览器上安装 Vue Devtools

第三步：阻止 vue 在启动时生成生产提示

```js
Vue.config.productionTip = false // 设置为 false 以阻止 vue 在启动时生成生产提示
```



## 1.3 第一个 Vue 应用

Vue.js 的核心是一个允许采用简洁的模板语法来声明式地将数据渲染进 DOM 的系统：

```html
<!-- 容器 -->
<div id="app">
  {{ message }}
</div>
```

```js
// Vue实例
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  }
})
```

打开浏览器控制台，修改 `app.message` 的值，会发现数据相应的更新

分析：

- el 是 Vue 实例对应的标签，与 css 中选择器语法相同

- data 是 Vue 实例的数据对象，message 对应容器中的 {{message}}
- 一夫一妻制：一个 Vue 实例对应一个容器





# 二、Vue 基础

## 2.1 Vue 实例

每个 Vue 应用都是通过 Vue 函数创建一个新的 Vue 实例开始：

```js
var vm = new Vue({
    
})
```

Vue 的设计受到 MVVM 模型的启发（类似于 MVC 架构模式的一种架构模式）

所以 Vue 实例一般命名为 vm（View Model）



### Vue 实例的属性

Vue 实例有许多属性，有的以 $ 开头，有的以 _ 开头

- $ 可以看作公开的属性，是供程序员使用的
- _ 可以看作私有的属性，Vue 底层使用的





## 2.2 模板语法

Vue.js 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据

### 插值

#### 文本

文本插值使用“Mustache”语法（双大括号）

```html
<span>Message:{{msg}}</span>
```

#### Attribute

上边文本插值的语法不能用在 HTML attribute 上，所以在对属性进行插值时，应使用 v-bind **指令**

以超链接为例：

```html
<!-- 这里 url 是变量名 -->
<a v-bind:href="url"></a>
```



### 指令

指令是带有 `v-` 前缀的特殊 attribute，当表达式的值改变时，产生的连带影响，响应式地作用于 DOM

- v-bind：绑定元素（单向数据绑定，缩写为 `:`）
- v-model:value：绑定元素（双向数据绑定，只能用在表单类元素上，简写为 `v-model`）
- v-on：监听事件（缩写为 `@`）
- v-once：只渲染元素一次，用来性能优化





## 2.3 事件处理

### 监听事件

使用 `v-on` 指令监听事件，缩写为 `@`

示例：

```html
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <!-- <button @click="counter += 1">Add 1</button> -->
  <p>The button above has been clicked {{ counter }} times.</p>
</div>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```



### 事件处理方法

许多事件处理很复杂，我们可以使用 `v-on` 调用方法，而不是直接在 js 代码写在指令中

注意：methods 中配置的函数，不要使用箭头函数！否则 this 不是当前的 Vue 实例

示例：

- 无参

  @click="method" 等价于 @click="method($event)"

  ```html
  <div id="example-2">
    <!-- `greet` 是在下面定义的方法名 -->
    <button v-on:click="greet">Greet</button>
  </div>
  ```

  ```js
  var example2 = new Vue({
    el: '#example-2',
    data: {
      name: 'Vue.js'
    },
    // 在 methods 对象中定义方法
    methods: {
      greet: function (event) {
        // this 在方法里指向当前 Vue 实例
        alert('Hello ' + this.name + '!')
        // event 是原生 DOM 事件
        if (event) {
          alert(event.target.tagName)
        }
      }
    }
  })
  
  // 也可以用 JavaScript 直接调用方法
  example2.greet() // => 'Hello Vue.js!'
  ```

- 带参

  ```html
  <div id="example-3">
    <button v-on:click="say('hi')">Say hi</button>
  </div>
  ```

  ```js
  new Vue({
    el: '#example-3',
    methods: {
      say: function (message) {
        alert(message)
      }
    }
  })
  ```

  有时需要访问原生的 DOM 事件，例如 event，使用 `$event` 传入方法即可

  ```html
  <button v-on:click="warn('Form cannot be submitted yet.', $event)">
    Submit
  </button>
  ```

  ```js
  // ...
  methods: {
    warn: function (message, event) {
      // 现在我们可以访问原生事件对象
      if (event) {
        event.preventDefault()
      }
      alert(message)
    }
  }
  ```



