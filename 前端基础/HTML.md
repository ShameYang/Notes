# HTML 概述

- 什么是 HTML

  HTML：Hyper Text Markup Language（超文本标记语言）

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

- 开发：文本编辑器或开发工具，文件扩展名为 .html 或 .htm

- 运行方式：浏览器打开 HTML 文件

- 规范制定方：W3C（世界万维网联盟）



# HTML 基本结构

```html
<!--
	1.这是 HTML 的注释
	2.HTML 不区分大小写，语法不严格
-->
<!--声明 HTML 5 语法，去掉就表示 HTML 4 语法-->
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



# 第一个 HTML（理解原理）

在概述中，我们提到了 HTML 的运行方式为浏览器打开

下述为完整的过程：（记事本方式，也可以用开发工具，更加方便）

1. 首先创建一个 .html 文件，用记事本打开，内容如下：

   ```html
   <!doctype html>
   <html>
   	<head>
   		<title>标题</title>
   	</head>
   	<body>
   		Hello,World!
   	</body>
   </html>
   ```

2. 保存并关闭，用浏览器打开 .html 文件，这样就完成了最简单的开发



# HTML 基本标签

```html
<html>
    <head>
        <title>HTML 基本标签</title>
    </head>
    
    <body>
		<p>段落标签</p>
        
		<!--标题-->
		<h1>一级标题</h1>
        ...
        <h6>六级标题</h6>
		
		<!--换行（单标签）-->
		换<br />行

		<!--水平线（单标签）-->
		<hr />
        
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

HTML 代码中需要用以下三种实体符号，表示小于号、大于号和空格：

- 小于号 `&lt;`
- 大于号 `&gt;`
- 空格 `&nbsp;`



# HTML 表格

- 表格 `<table></table>`

- 一行 `<tr></tr>`

- 表头单元格 `<th></th>`

- 一个单元格 `<td></td>`

例如，下边是一个 3行2列 的表格：

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

- 头 `<thead></thead>`
- 体 `<tbody></tbody>`
- 脚 `<tfoot></tfoot>`

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



# HTML 图片标签

`<img src="图像路径" />`

| **属性** | 属性值   | 说明                                         |
| -------- | -------- | -------------------------------------------- |
| src      | 图片路径 | 必须属性                                     |
| width    | 像素     | 设置图像的宽度，只设置宽度时，高度等比例缩放 |
| height   | 像素     | 设置图像的高度                               |
| title    | 文本     | 鼠标悬停时显示的信息                         |
| alt      | 文本     | 图片无法加载时的提示信息                     |
| border   | 像素     | 设置图片边框粗细                             |



# HTML 超链接

超链接的作用：

通过超链接，可以从浏览器向服务器发送请求

- 浏览器向服务器发送请求（请求，request）
- 服务器向浏览器发送数据（响应，response）

B / S 结构的系统，每个请求都对应一个响应

```html
<!--
	href 是必须属性，用于指定链接目标的url地址
	target 用于指定链接页面的打开方式（_self 为默认值:当前窗口打开页面，_blank 在新窗口打开页面）
-->
<a href="跳转目标" target="目标窗口的弹出方式">文本或图像</a>
```



# HTML 列表

无序列表

```html
<ul>
    <li>列表项1</li>
    	<!--可以嵌套-->
    	<ul>
            <li></li>
    	</ul>
    <li>列表项2</li>
    ...
</ul>
```

有序列表

```html
<ol>
   <li>列表项1</li>
    	<!--可以嵌套-->
    	<ol>
            <li></li>
    	</ol>
   <li>列表项2</li>
   ...
</ol>
```



# HTML 表单

表单用来收集用户的信息，最终将数据提交给服务器

格式：

- action?name=value&name=value&...

  以上格式是 HTTP 协议规定的，必须以这种格式提交给服务器

表单域：

- `<form action="指定数据提交的地方" method="提交信息是否显示在地址栏(get / post)"></form>`

表单元素（表单控件）：

- `<input type="属性值"/>`

  | 属性值   | 说明                                                   |
  | -------- | ------------------------------------------------------ |
  | button   | 普通按钮，不能提交表单                                 |
  | submit   | 提交按钮，把表单数据提交到服务器                       |
  | reset    | 重置按钮，清空表单数据                                 |
  | password | 密码框                                                 |
  | radio    | 单选框，相同 name 可以实现多选一                       |
  | checkbox | 复选框                                                 |
  | text     | 单行的输入框                                           |
  | file     | 文件上传专用                                           |
  | hidden   | 隐藏域，浏览器上不显示，但是设置 name 后会提交到服务器 |

  input 其他属性

  | 属性      | 属性值     | 说明                                   |
  | --------- | ---------- | -------------------------------------- |
  | name      | 用户自定义 | input 元素的名字，有 name 数据才能提交 |
  | value     | 用户自定义 | 按钮上显示的文本                       |
  | checked   | checked    | 首次加载时就被选中                     |
  | readonly  | readonly   | 只读，但是可以上传到服务器             |
  | disabled  | disabled   | 只读，不能上传到服务器                 |
  | maxlength | 整数       | 设置文本框可输入的字符数量             |

- 下拉选项：`<select></select>`

  ```html
  <select>
      <!--selected 表示默认选项-->
      <option>选项1</option>
      <option selected>选项2</option>
      ...
  </select>
  ```

- 文本域：`<textarea></textarea>`

  文本域没有 value 属性

例：用户注册表单

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>用户注册表单</title>
	</head>
	<body>
		<!-- 
			用户名
			密码
			确认密码
			性别
			兴趣爱好
		 -->
		<form action="http://localhost:8080/test" method="post">
            <input type="hidden" name="userid" value="111" />
			用户名
			<input type="text" name="username"/>
			<br />
			密码
			<input type="password" name="userpwd" />
			<br />
			确认密码
			<input type="password" />
			<br />
			性别
			<input type="radio" name="gender" />男
			<input type="radio" name="gender" />女
			<br />
			兴趣爱好
			<input type="checkbox" name="interest" value="sing" checked/>唱
			<input type="checkbox" name="interest" value="jump" />跳
			<input type="checkbox" name="interest" value="rap" />rap
			<input type="checkbox" name="interest" value="basketball" />篮球
			<br />
			学历
			<select name="grade">
				<option value="zk">专科</option>
				<option value="bk" selected>本科</option>
				<option value="ss">硕士</option>
			</select>
			<br />
			简介
			<textarea name="introduce" id="" cols="20" rows="5"></textarea>
			<br />
			<input type="submit" value="注册" />
			<input type="reset" value="清空" />
		</form>
	</body>
</html>
```



# HTML 中的 id 属性

在 HTML 文档中，任何元素（节点）都有 id 属性，id 属性是节点的唯一标识，所以同一 HTML 文档中 id 值不能重复

表单提交数据时，只和 name 有关，和 id 无关

- id 作用：
  - JS 可以对 HTML 文档中任何节点进行增删改操作
  - id 让我们获取节点更方便

DOM(Document) 树：HTML 文档是树状结构，每个节点都有唯一的 id



# HTML 中 div 和 span

div 和 span 是什么？有什么用？

- div 和 span 都可以称为“图层”
- 作用是保证页面可以灵活布局
- 图层就是一个一个的盒子

最早的网页采用 table 布局，现在 div 布局使用最多，灵活布局

div 独占一行，span 不会

```html
<div></div>
<span></span>
```

