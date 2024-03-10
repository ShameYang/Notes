# 一、Vue 简介

## 1.1 什么是 Vue？

Vue 是一款用于构建用户界面的 JavaScript 框架，它基于标准 HTML、CSS 和 JavaScript 构建，并提供了一套声明式的、组件化的编程模型，帮助你高效地开发。

- 声明式渲染：Vue 基于标准 HTML 拓展了一套模板语法，使得我们可以声明式地描述最终输出的 HTML 和 JavaScript 状态之间的关系。
- 响应性：Vue 会自动跟踪 JavaScript 状态并在其发生变化时响应式地更新 DOM。

## 1.2 渐进式框架

Vue 适用不同的需求，无论是简单项目还是复杂项目，所以被称作 “渐进式框架”。

## 1.3 单文件组件

单文件组件，也就是 `*.vue` 文件。在使用 Vue 的项目中，可以用一种类似 HTML 格式的文件来书写 Vue 组件。

Vue 的单文件组件会将一个组件的逻辑 (JavaScript)，模板 (HTML) 和样式 (CSS) 封装在同一个文件里。



# 二、快速上手

## 2.1 基于 vite 创建工程

前提：

- 熟悉命令行
- 已安装 18.0 或更高版本的 Node.js

① 命令行中执行以下命令（不要带 `$` 符号）：

```sh
$ npm create vue@latest
```

该指令会安装并执行 `create-vue`，它是 Vue 官方的项目脚手架工具。

输入项目名称之后，如果不确定是否要开启某个功能，可以一直回车（选择 `No`）。

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/first.png" style="zoom: 80%; float:left;" />

② 项目创建后，安装依赖并启动开发服务器：

```sh
$ cd <your-project-name>
$ npm install
$ npm run dev
```

当你准备将应用发布到生产环境时，请运行：

```sh
$ npm run build
```

此命令会在 `./dist` 文件夹中为你的应用创建一个生产环境的构建版本。



# 三、Vue 基础

## 3.1 创建一个应用

### 应用实例

每个 Vue 应用都是通过 `createApp` 函数创建一个新的 **应用实例**：

```js
import { createApp } from 'vue'

const app = createApp({
   /* 根组件选项 */
})
```

### 根组件

上边代码中，我们传入 `createApp` 的对象是一个组件，每个应用都需要一个“根组件”，其它组件作为子组件。

如果使用的是单文件组件，可以直接从另一个文件中导入根组件：

```js
import {createApp} from 'vue'
// 从一个单文件组件中导入根组件
import App from './App.vue'

const app = createApp(App)
```

### 挂载应用

应用实例在调用了 `.mount()` 方法后才会渲染出来。该方法接收一个“容器”参数，可以是一个实际的 DOM 元素或是一个 CSS 选择器字符串：

```html
<div id="app"><div>
```

```js
app.mount('#app')
```

### 多个应用实例

应用实例可以共存，每个应用都拥有自己的用于配置和全局资源的作用域。

```js
const app1 = createApp({
  /* ... */
})
app1.mount('#container-1')

const app2 = createApp({
  /* ... */
})
app2.mount('#container-2')
```

## 3.2 模板语法

Vue 使用了基于 HTML 的模板语法，使我们能够声明式地将其组件实例的数据绑定到呈现的 DOM 上。

### 文本插值

文本插值是最基本的数据绑定形式，将数据解释为纯文本，使用“Mustache”语法（双大括号）：

```html
<span>Message:{{msg}}</span>
```

双大括号标签会被替换为相应组件实例中 `msg` 属性的值。同时每次 `msg` 属性更改时它也会同步更新。

### 原始 HTML

如果想插入 HTML，需要使用 `v-html` 指令。

例如：

```html
// 在当前组件实例上，将此元素的 innerHTML 与 rawHtml 属性保持同步
// span 的内容将会被替换为 rawHtml 属性的值
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

### 指令

在插入 HTML 时遇到了新的概念：指令。指令由 `v-` 作为前缀，为渲染的 DOM 应用特殊的响应式行为。

### Attribute 绑定

双大括号不能在 HTML 的属性中使用。想要响应式地绑定一个 atrribute，需要使用 `v-bind` 指令：

```vue
<div v-bind:id="dynamicId"></div>

// 简写
<div :id="dynamicId"></div>

// 同名简写（Vue 3.4 及以上版本可用）
<!-- 与 :id="id" 相同 -->
<div :id></div>

// 布尔型 Attribute
<button :disabled="isButtonDisabled">Button</button>

// 动态绑定多值
const objectOfAttrs = {
  id: 'container',
  class: 'wrapper'
}

<div v-bind="objectOfAttrs"></div>
```

`v-bind` 指令指示 Vue 将元素的 `id` attribute 与组件的 `dynamicId` 属性保持一致。如果绑定的值是 `null` 或者 `undefined`，那么该 attribute 将会从渲染的元素上移除。

### 使用 JS 表达式

使用场景：

- 文本插值中
- 任何 Vue 指令 attribute 的值中

```html
{{ number + 1 }}

{{ ok ? 'YES' : 'NO' }}

{{ message.split('').reverse().join('') }}

<div :id="`list-${id}`"></div>
```

## 3.3 响应式基础

### 声明响应式状态

#### 推荐：ref()

在组合式 API 中，推荐使用 [`ref()`](https://cn.vuejs.org/api/reactivity-core.html#ref) 函数来声明响应式状态：

```js
import { ref } from 'vue'

const count = ref(0)
```

#### \<script setup>

在 `setup()` 函数中手动暴露大量的状态和方法非常繁琐：

```js
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)

    function increment() {
      // 在 JavaScript 中需要 .value
      count.value++
    }

    // 不要忘记同时暴露 increment 函数
    return {
      count,
      increment
    }
  }
}
```

```html
<button @click="increment">
  {{ count }}
</button>
```

我们可以使用 `<script setup>` 来大幅度地简化代码：

```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">
    {{ count }}
  </button>
</template>
```

#### DOM 更新时机

当你修改了响应式状态时，DOM 会被自动更新。但是需要注意的是，DOM 更新不是同步的。Vue 会在“next tick”更新周期中缓冲所有状态的修改，以确保不管你进行了多少次状态修改，每个组件都只会被更新一次。

要等待 DOM 更新完成后再执行额外的代码，可以使用 [nextTick()](https://cn.vuejs.org/api/general.html#nexttick) 全局 API：

```js
import { nextTick } from 'vue'

async function increment() {
  count.value++
  await nextTick()
  // 现在 DOM 已经更新了
}
```

#### reactive()

reactive() API 是另一种声明响应式状态的方式。

与将内部值包装在特殊对象中的 ref 不同，`reactive()` 将使对象本身具有响应性：

```js
import { reactive } from 'vue'

const state = reactive({ count: 0 })
```

模板中使用：

```html
<button @click="state.count++">
  {{ state.count }}
</button>
```

局限性

- **有限的值类型**：它只能用于对象类型 (对象、数组和 Map、Set 这样的集合类型)。它不能持有如 string、number 或 boolean 这样的原始类型。

- **不能替换整个对象**：由于 Vue 的响应式跟踪是通过属性访问实现的，因此我们必须始终保持对响应式对象的相同引用。

- **对解构操作不友好**：当我们将响应式对象的原始类型属性解构为本地变量时，或者将该属性传递给函数时，我们将丢失响应性连接：

  ```js
  const state = reactive({ count: 0 })
  
  // 当解构时，count 已经与 state.count 断开连接
  let { count } = state
  // 不会影响原始的 state
  count++
  
  // 该函数接收到的是一个普通的数字
  // 并且无法追踪 state.count 的变化
  // 我们必须传入整个对象以保持响应性
  callSomeFunction(state.count)
  ```

  
