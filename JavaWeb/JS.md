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







# HTML 中嵌入 JS 代码

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







# JS 的变量

> JS 是一种弱类型语言，没有编译阶段，一个变量可以随意赋值

- 声明方式

  `var 变量名;`

- 赋值

  `变量名 = 值;`