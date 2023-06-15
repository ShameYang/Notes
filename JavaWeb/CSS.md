# CSS 概述

- 什么是 CSS

  CSS：Cascading Style Sheet（层叠样式表）

- CSS 的作用

  CSS 用来修饰 HTML 页面，设置 HTML 页面中的某些元素

  HTML 是主体，CSS 依赖 HTML，新建的文件仍然是 .html 文件







# CSS 的使用方式

1. 内联定义

   在标签内部使用 style 属性设置元素的 CSS 样式

   语法格式：`<标签 style="样式名:样式值; 样式名:样式值; ..."></标签>`

2. 样式块

   在 head 标签中，使用 style 块

   语法格式

   ```html
   <head>
       <style type="text/css">
           选择器 {
               样式名:样式值;
               样式名:样式值;
               ...
           }
       </style>
   </head>
   ```

3. 链入外部样式表文件（最常用的方式）

   将样式写入 .css 文件中，在需要的网页引入 css 文件，方便维护

   语法格式：`<link type="text/css" rel="stylesheet" href="css 文件的路径" />`







# CSS 选择器

1. id 选择器

   语法格式

   ```
   #id {
   	样式名:样式值;
   	样式名:样式值;
   	...
   }
   ```

2. 标签选择器

   语法格式

   ```
   标签名 {
   	样式名:样式值;
   	样式名:样式值;
   	...
   }
   ```

3. 类选择器

   语法格式：注意 . 不能丢

   对应的标签设置 class 属性

   ```
   .类名 {
   	样式名:样式值;
   	样式名:样式值;
   	...
   }
   ```







# 边框

```css
选择器 {
	border: 1px solid red;
}

等价于

选择器 {
	border-width: 1px;
	border-style: solid;
	border-color: red;
}
```







# 隐藏

```css
选择器 {
	display: none;
}
```







# 字体

```css
选择器 {
	font-size: 12px;
	color: red;
}
```







# 文本装饰

```css
选择器 {
	text-decoration: none; /* 无装饰，例如可以取消超链接的下划线 */
}
```







# 鼠标悬停效果

```css
选择器 {
	cursor: pointer; /* pointer 是小手，hand 也是，但是 hand 存在浏览器兼容问题 */
}
```







# 列表样式

```
选择器 {
	list-style-type: none; /* 去掉列表前的符号 */
}
```







# 定位

```
选择器 {
	position: absolute; /* 绝对定位 */
    /* 距离上下左右都是 10px */
    top: 10px;
    left: 10px;
    bottom: 10px;
    right: 10px;
}
```

