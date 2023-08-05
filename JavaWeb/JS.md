# JavaScript 概述

- 什么是 JavaScript

  JavaScript 是运行在浏览器上的脚本语言，简称 JS

  由网景公司（NetScape）的布兰登艾奇开发，最初叫作 LiveScript

  与 Sun 公司合作后，改名为 JavaScript

  JS 与 Java 没有任何关系，只是语法上有些类似

- JS 的作用

  JS 可以是页面动态化，更具交互性

- JS 与 JSP 的区别

  JS：JavaScript（运行在浏览器上）

  JSP：JavaServer Pages（隶属于 Java 语言，运行在 JVM 上）







# JavaScript 三部分

1. ECMAScript：JS 的核心语法（ES 规范 / ECMA -262标准）
2. DOM：Document Object Model（文档对象模型），HTML 文档被看作一颗 DOM 树
3. BOM：Browser Object Model（浏览器对象模型），关闭浏览器窗口、打开一个新的浏览器窗口、后退、前进、地址栏等，都是 BOM 编程







# ECMAScript

## HTML 中嵌入 JS 代码

> JS 是一门事件驱动型的编程语言，依靠事件驱动，然后执行对应的程序
>
> - JS 中有很多事件，其中有一个事件叫做 click（鼠标单击）
>
> - 每个事件都会对应一个事件句柄（事件单词前加 on），例如 click 的事件句柄为 onclick
>
> - 事件句柄以 HTML 标签的属性存在



click 示例

1. 第一种方式：作为标签属性

   ```html
   <!doctype html>
   <html>
   	<head>
           <title>JS 嵌入 HTML 第一种方式</title>
   	</head>
   	<body>
   		<!--
   			1.onclick="JS 代码" 执行原理：
   			页面打开时，JS 代码注册到 click 事件上，发生事件后，JS 代码被浏览器自动调用
   
   			2.JS 弹窗：使用 JS 内置对象 window 调用 alert 函数，window.alert("弹窗消息")，window. 可以省略不写
   		-->
   		<input type="button" value="我是按钮" onclick="window.alert('hello js');" />
           
           <input type="button" value="我是按钮" onclick="alert('hello js');" />
           
           <input type="button" value="我是按钮3" onclick="alert('aaa'); alert('bbb'); alert('ccc');" />
           
   	</body>
   </html>
   ```

   

2. 第二种方式：脚本块

   脚本块出现的次数和位置没有要求

   ```html
   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title>JS 嵌入 HTML 第二种方式</title>
   	</head>
   	<body>
   		<!-- 第二种方式：脚本块 -->
   		<script type="text/javascript">
               // 脚本块中的 JS 代码，会在打开页面时执行，遵循自上而下的顺序逐行执行
               // alter 函数会阻塞页面加载
   			window.alert("Hello World");
   		</script>
   	</body>
   </html>
   ```

   

3. 第三种方式：引入外部文件

   ```html
   <!DOCTYPE html>
   <html>
   	<head>
   		<meta charset="utf-8">
   		<title>JS 嵌入 HTML 第三种方式：引入外部文件</title>
   	</head>
   	<body>
   		<script type="text/javascript" src="test.js"></script>
   	</body>
   </html>
   ```

   根目录下 js 文件

   ```js
   window.alert("hello js!!");
   ```







## 变量、函数

### 声明和赋值

> JS 是一种弱类型语言，没有编译阶段，一个变量可以随意赋值

- 声明方式

  `var 变量名;`

- 赋值

  `变量名 = 值;`





### 函数

> 由于 JS 为弱类型语言，函数不能重载，后声明的函数会覆盖之前的同名函数

语法格式：

```
//第一种方式
function 函数名(形参列表) {
	函数体;
}

//第二种方式
函数名 = function(形参列表) {
	函数体;
}
```





### 全局变量和局部变量

- 全局变量

  在函数体之外声明的变量

  生命周期：浏览器打开时声明，关闭时销毁

- 局部变量

  在函数体当中声明的变量，包括函数的形参

  生命周期：函数开始执行时，局部变量的内存空间开辟，函数执行结束后，内存空间释放



注意：

1. 全局变量会一直耗费浏览器内存空间，尽量使用局部变量
2. 当变量声明时没有使用 var 关键字，那么该变量为全局变量







## 数据类型

> JS 中数据类型有原始类型和引用类型

- 原始类型
  - Undefined
  - Number
  - String
  - Boolean
  - Null
- 引用类型：Object 以及 Object 子类



## typeof 运算符

> typeof 可以在程序运行时动态获取变量的数据类型

- 语法格式：`typeof 变量名`
- 运算结果："undefined"、"number"、"string"、"boolean"、"object"、"function"

- 在 JS 中比较字符串使用 ==

  == 判断值，=== 判断值和数据类型





## Object

类的定义与函数定义格式一样

例如

```js
function Student(sname, gender) {
    //属性
    this.sname = sname;
    this.gender = gender;
}
```

- 调用属性：

  `类.属性`(与 java 一致)；还可以`类["属性"]`



- Object 类中有 prototype 属性，可以使用该属性动态扩展类







## 事件

常用事件

- 焦点
  - blur 失去焦点
  - focus 获得焦点
- 鼠标点击
  - click 鼠标单击
  - dbclick 鼠标双击
- 键盘
  - keydown 键盘按下
  - keyup 键盘弹起
- 鼠标
  - mouseover 鼠标经过
  - mousemove 鼠标移动
  - mouseout 鼠标离开
  - mouseup 鼠标弹起
- 表单
  - reset 重置
  - submit 提交
- change 下拉列表选项改变，或文本框内容改变
- select 文本被选定
- load 页面加载完毕



每个事件都对应一个事件句柄，事件句柄 = 事件前添加 on（onXXX ）

事件句柄以属性的形式存在



注册事件的两种方式：

```html
<!-- 第一种方式，html 标签中 -->
<标签 onXXX="回调函数()"></标签>


<!-- 第二种方式，纯 js -->
<script type="text/javascript">
	//1.获取对象
	//document 为内置对象，代表整个 html 页面
	var obj = document.getElementById("对应的 id");
	//2.给对象的 onXXX 属性赋值
	obj.onXXX = 回调函数; //这里的函数不加小括号
</script>
```







## 捕捉回车键

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>JS 捕捉回车键</title>
	</head>
	<body>
		<script type="text/javascript">
			window.onload = function() {
				var username = document.getElementById("mytext");
				username.onkeydown = function(event) {
					if (event.keyCode == 13) {
						alert("正在验证...");
					}
				}
			}
		</script>

		<input type="text" id="mytext">
	</body>
</html>
```







## void 运算符

语法：`void(表达式)`

运算原理：执行表达式，但不返回任何结果

`javascript:void(0)`，javascript: 作用是告诉浏览器后面是一段 JS 代码



```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>void 运算符</title>
	</head>
	<body>
		<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>		<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>
		<a href="javascript:void(0)" onclick="window.alert('test code')">
			既保留超链接样式，同时用户点击超链接时执行一段 JS 代码，页面不跳转
		</a>
	</body>
</html>
```









## 控制语句

JS 中控制语句大部分和 Java 中相同，多了 for...in 和 with（了解即可）

```js
//for...in
//1.遍历数组
var arr = [false, true, 1, 2, "abc", 3.14];
for (var i in arr) {
    alter(arr[i]);
}
//2.遍历对象属性
User = function(username, password) {
    this.username = username;
    this.password = password;
}
var u = new User("tom", 123);
for (var property in u) {
    alert(u[property]);
}
```







# DOM 和 BOM 的区别和联系

BOM 包括 DOM

BOM 的顶级对象：window

DOM 的顶级对象：document







# DOM

## 获取文本框 value

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>DOM编程-获取文本框的value</title>
		<script type="text/javascript">
			window.onload = function() {
				var btnElt = document.getElementById("btn");
				btnElt.onclick = function() {
					alert(document.getElementById("username").value);
				}
			}
		</script>
	</head>
	<body>
		<input type="text" id="username">
		<input type="button" value="获取文本框 value" id="btn">
	</body>
</html>
```







## innerHTML 和 innerText 操作div和span

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>DOM编程-innerHTML和innerText操作div和span</title>
		<style type="text/css">
			#div {
				background-color: aquamarine;
				width: 300px;
				height: 300px;
				border: 1px black solid;
				position: absolute;
				top: 100px;
			}
		</style>
	</head>
	<body>
		<script type="text/javascript">
			window.onload = function() {
				var btnElt = document.getElementById("btn");
				btnElt.onclick = function() {
					var divElt = document.getElementById("div");
					// divElt.innerHTML = "<font color='red'>innerHTML</font>";
					divElt.innerText = "<font color='red'>innerText</font>";
				}
			}
		</script>

		<input type="button" value="设置 div 中的内容" id="btn">

		<div id="div"></div>
	</body>
</html>
```

