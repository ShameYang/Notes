# 系统结构介绍

B / S 架构

> Browser / Server（浏览器 / 服务器的交互形式）
>
> 支持语言：HTML、CSS、JavaScript
>
> 我们主要使用 Java 语言完成服务端的开发
>
> 
>
> B / S架构
>
>   优点：升级方便，只升级服务端代码即可。维护成本低
>
>   缺点：速度慢、体验不好、界面不炫酷
>
> 企业大多采用 B / S 架构
>
> 
>
> 常见 B / S 架构系统：京东、淘宝、百度...



C / S 架构

> Client / Server（客户端 / 服务器端的交互形式）
>
> 
>
> 优点：速度快，体验好，界面炫酷
>
> 缺点：升级麻烦，维护成本高
>
> 
>
> 常见的 C / S 架构系统：QQ、微信、支付宝...







# HTML 概述

- 什么是 HTML

  HTEML：Hyper Text Markup Language（超文本标记语言）

  超文本：流媒体、图片、视频...

  由大量的标签组成，每个标签都有开始标签和结束标签

  ```
  <标签>
  	<标签>
  		<标签 属性名="属性值">
  		</标签>
  	</标签>
  </标签>
  ```

- 开发：文本编辑器或专业开发工具，文件扩展名为 .html 或 .htm

- 运行方式：浏览器打开 HTML 文件就是运行

- 规范由谁制定：W3C（世界万维网联盟）







# HTML 基本结构

```html
<!--
	1.这是 HTML 的注释
	2.HTML 不区分大小写，语法不严格
	3.加上下边第一行代码表示 HTML 5 语法，去掉就表示 HTML 4 语法
-->
<!doctype html>

<!--根-->
<html>

	<!--网页头部-->
	<head>
		<title>网页标题</title>
	</head>
	
	<!--网页主体内容-->
	<body>
		Hello,World!
	</body>
</html>

```







# HTML 基本标签

```html
<html>
    <head>
        <title>HTML 基本标签</title>
    </head>
    
    <body>
		<p>段落标签</p>
        
		<!---标题->
		<h1>一级标题</h1>
        <h6>六级标题</h6>
		
		<!--换行（单标签）-->
		换<br>行

		<!--水平线（单标签）-->
		<hr>
        
        <!--预留格式-->
		<pre>for (int i = 0; ;) {
			...
		}</pre>
        
        <strong>加粗</strong>  <b>另一种加粗</b>
        <em>倾斜</em>  <i>另一种倾斜</i>
        <del>删除字</del>
        <ins>插入字</ins>
        10<sup>2</sup>
        10<sub>m</sub>
        
        <!--字体标签-->
        <font color="red">字体标签</font>
    </body>
</html>
```







# HTML 实体符号

- 小于号`&lt;`
- 大于号`&gt;`
- 空格`&nbsp;`

HTML 代码中需要用以上三种实体符号表示小于号、大于号和空格







# HTML 表格

- 表格`<table></table>`

- 一行`<tr></tr>`

- 表头单元格`<th></th>`

- 一个单元格`<td></td>`



例如下边是一个 3行2列 的表格

```html
<!--
	align="center" 表格居中
	border="1px" 设置表格的边框为1像素宽度
	width="300px" 设置表格宽度为300像素
	height="20%" 设置表格高度为窗口高度的20%
-->
<table align="center" border="1px" width="300px" height="20%">
    <!--align 对齐方式-->
    <tr align="center">
        <!--th 比 td 多的是居中和加粗-->
        <th>1</th>
        <td align="center">2</td>
    </tr>
    <tr>
        <td>a</td>
        <td>b</td>
    </tr>
    <tr>
        <td>x</td>
        <td>y</td>
    </tr>
</table>
```



## 合并单元格

- rowspan：合并行

  例如：合并 b 和 y

  ```html
  <!--注意：rowspan 合并行时，要删掉下边的单元格-->
  <table border="1px" width="300px" height="20%">
      <tr>
          <td>1</td>
          <td>2</td>
      </tr>
      <tr>
          <td>a</td>
          <td rowspan="2">b</td>
      </tr>
      <tr>
          <td>x</td>
          <!--<td>y</td>-->
      </tr>
  </table>
  ```

- colspan：合并列

  例如：合并 1 和 2

  ```html
  <!--colspan 合并列时，对删除的列没有要求，因为两列都在同一行-->
  <table border="1px" width="300px" height="20%">
      <tr>
          <td colspan="2">1</td>
          <!--<td>2</td>-->
      </tr>
      <tr>
          <td>a</td>
          <td>b</td>
      </tr>
      <tr>
          <td>x</td>
          <td>y</td>
      </tr>
  </table>
  ```




## 表格结构标签

> 在 table 中不是必须的，这样写方便后期 JS 代码

- 头`<thead></thead>`
- 体`<tbody></tbody>`
- 脚`<tfoot></tfoot>`

```html
<table border="1px" width="300px" height="20%">
    <thead>
        <tr>
        	<td>1</td>
        	<td>2</td>
    </tr>
    </thead>
    <tbody>
    	<tr>
        	<td>a</td>
        	<td>b</td>
    	</tr>
    </tbody>
    <tfoot>
        <tr>
        	<td>x</td>
        	<td>y</td>
    	</tr>
    </tfoot>
</table>
```

