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