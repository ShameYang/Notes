# 传统请求及缺点

- 传统的请求有哪些？
  - 直接在浏览器地址栏输入 URL
  - 点击超链接
  - 提交 form 表单
  - 使用 JS 代码发送请求
    - window.open(url)
    - document.location.href = url
    - window.location.href = url
    - ...
- 存在的问题：
  - 页面会全部刷新，导致用户体验差
  - 传统请求会使用户体验不连贯







# 关于 AJAX

- Asynchronous JavaScript And XML（异步的 JavaScript 和 XML）
  - 什么是异步？
    - 假设有线程1和线程2，两线程并发，就是异步
- AJAX 不是新的编程语言，而是一种使用现有标准的新方法，解决了传统请求存在的问题
- AJAX 代码属于 JS 代码
- AJAX 最大的优点：可以在不重新加载页面的情况下，与服务器交换数据并更新部分内容（局部刷新）







# XMLHttpRequest 对象

- XMLHttpRequest 对象是 AJAX 的核心对象，负责发送请求和接收服务器返回的数据

- 目前的浏览器都内置了该对象

- 创建 XMLHttpRequest 对象

  ```js
  var xhr = new XMLHttpRequest();
  ```

- 相关方法

  | 方法                                | 描述                                                         |
  | ----------------------------------- | ------------------------------------------------------------ |
  | abort()                             | 取消当前请求                                                 |
  | getAllResponseHeaders()             | 返回头部信息                                                 |
  | getResponseHeader()                 | 返回特定的头部信息                                           |
  | open(method, url, async, user, psw) | 规定请求 method：请求方式   url：文件位置   async：异步或同步   user：可选的用户名称   pwd：可选的密码 |
  | send()                              | 将请求发送到服务器，用于 GET 请求                            |
  | send(string)                        | 将请求发送到服务器，用于 POST 请求                           |
  | setRequestHeader()                  | 向要发送的报头添加标签/值对                                  |

- 相关属性

  | 属性               | 描述                                                         |
  | ------------------ | ------------------------------------------------------------ |
  | onreadystatechange | 定义当 readyState 属性发生变化时被调用的函数                 |
  | readyState         | 保存 XMLHttpReqeust 对象的状态。0：请求未初始化   1：服务器连接已建立   2：请求已收到   3：正在处理请求   4：请求已完成且响应已就绪 |
  | responseText       | 以字符串返回响应数据                                         |
  | responseXML        | 以 XML 数据返回响应数据                                      |
  | status             | 返回请求的状态号。200："OK"   403："Forbidden"   404："Not Found" |
  | statusText         | 返回状态文本（如："OK" 或 "Not Found"）                      |







# AJAX GET 请求

示例代码：

- ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>AJAX get请求</title>
      <script type="text/javascript">
          window.onload = function () {
              document.getElementById("myBtn").onclick = function () {
                  // 第一步：创建 AJAX 核心对象
                  const xhr = new XMLHttpRequest();
                  // 第二步：注册回调函数
                  xhr.onreadystatechange = function () {
                      if (this.readyState === 4) {
                          if (this.status === 200) {
                              document.getElementById("myDiv").innerHTML = this.responseText;
                          } else {
                              alert(this.status)
                          }
                      }
                  }
                  // 第三步：开启通道
                  xhr.open("GET", "/ajax/ajaxrequest01", true);
                  // 第四步：发送请求
                  xhr.send();
              }
          }
      </script>
  </head>
  <body>
      <!-- 用户点击按钮时，发送 ajax 请求 -->
      <button id="myBtn">发送 ajax 请求</button>
      <!--  ajax 接收响应的数据后，对 div 进行渲染  -->
      <div id="myDiv"></div>
  </body>
  </html>
  ```







# AJAX GET 请求的缓存问题

- 对于低版本的浏览器来说，AJAX 的 get 请求可能会走缓存，存在缓存问题
- AJAX 请求缓存问题
  - HTTP 协议中规定：get 请求会被缓存起来
  - 发送 AJAX GET 请求时，对于低版本的浏览器来说，第二次的请求会走缓存，不走服务器
- POST 请求不会被浏览器缓存起来
- GET 请求缓存的优缺点
  - 优点：直接从浏览器缓存中获取资源，不需要从服务器上重新加载，速度快，用户体验好
  - 缺点：无法实时获取最新的服务器资源
- 浏览器什么时候走缓存？
  - GET 请求
  - 请求路径被浏览器缓存了，第二次发送请求时，路径没有变化
- 低版本的浏览器怎么解决 AJAX GET 请求的缓存问题？
  - 在请求路径 url 后边添加时间戳或随机数，这样每次的路径都会发生改变
  - "url?t=" + new Date().now()
  - "url?t=" + Math.random()







# AJAX POST 请求

- 与 GET 请求的区别：

  - 只有前端代码有区别

  - post 请求提交的数据在 send 方法中，而 get 请求提交的数据在 open 中
  - post 请求需要设置请求头的内容类型，get 请求不需要

- 示例代码

  ```html
  <!DOCTYPE html>
  <html lang="en">
  <head>
      <meta charset="UTF-8">
      <title>ajax post</title>
      <script type="text/javascript">
          window.onload = function () {
              document.getElementById("myBtn").onclick = function () {
                  // 第一步：创建 AJAX 核心对象
                  const xhr = new XMLHttpRequest();
                  // 第二步：注册回调函数
                  xhr.onreadystatechange = function () {
                      if (this.readyState === 4) {
                          if (this.status === 200) {
                              document.getElementById("mySpan").innerHTML = this.responseText;
                          } else {
                              alert(this.status);
                          }
                      }
                  }
                  // 第三步：开启通道
                  xhr.open("POST", "/ajax/ajaxrequest02", true);
                  // 设置请求头的内容类型
                  xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
                  // 第四步：发送请求
                  const username = document.getElementById("username").value;
                  const password = document.getElementById("password").value;
                  xhr.send("username=" + username +"&password="+ password);
              }
          }
      </script>
  </head>
  <body>
      用户名：<input type="text" id="username" name="username"><br>
      密码：<input type="password" id="password" name="password"><br>
      <button id="myBtn">发送 AJAX 请求</button><br>
      <span id="mySpan"></span>
  </body>
  </html>
  ```

- 案例：使用 AJAX POST 请求实现验证用户名
  - 步骤：
    - 前端：用户输入用户名后，失去焦点事件 blur 发生，然后发送 AJAX POST 请求，提交用户名
    - 后端：接收用户名后，连接数据库，查询是否可用 







# 基于 JSON 的数据交换

- 在 WEB 前端中，如何将一个 json 格式的字符串转换为 json 对象

  - eval 函数

  - 内置对象 JSON 的 parse 方法

    ```js
    var jsonStr = "{\"username\" : \"xxx\"}";
    var jsonObj = JSON.parse(jsonStr);
    ```







## fastjson

- 使用阿里巴巴的 fastjson 组件可以非常便利地将 java 对象转换成 json 格式的字符串

  ```java
  String jsonStr = JSON.toJSONString();
  ```

- 注意：需要导入 fastjson 的 jar 包







# 基于 XML 的数据交换

- 注意：如果服务器端响应 XML 的话，需要修改响应的内容类型

  ```java
  response.setContentType("text/xml;charset=UTF-8");
  ```







# AJAX 乱码问题

- Tomcat 9以及之前版本，需要设置字符集，否则会出现中文乱码问题

- 解决方案：

  - 响应时乱码

    ```java
    response.setContentType("text/html;charset=UTF-8");
    ```

  - 发送 AJAX post 请求时，服务器接收乱码

    ```java
    request.setCharacterEncoding("UTF-8");
    ```







# AJAX 异步和同步

- 什么是异步？什么是同步？
  - 异步：ajax 请求1和 ajax 请求2，同时并发，谁也不用等谁
  - 同步：ajax 请求1在发送时，需要等待 ajax 请求2结束之后才能发送

- 什么时候用同步？
  - 根据具体业务，例如用户注册
    - 只有验证用户名等请求完成后，才能发送注册请求，这种情况下就要使用同步







# AJAX 代码封装到 jQuery 库

- 手动封装一个工具类，这个工具类看作是一个 JS 的库，这个 JS 库就是 jQuery

  - 与后端没有关系，封装的 jQuery 只是为了方便前端代码的编写

  - 设计思路来源于 CSS 的语法

  - 手动开发 jQuery 源码：

    ```js
    function jQuery(selector) {
        if (typeof selector == "string"){
            if (selector.charAt(0) === "#") {
                // 全局变量
                domObj = document.getElementById(selector.substring(1));
                return new jQuery();
            }
        }
        // window.onload
        if (typeof selector == "function") {
            window.onload = selector;
        }
        // 定义 html() 函数，代替 domObj.innerHTML = ""
        this.html = function(htmlStr) {
            domObj.innerHTML = htmlStr;
        }
        // 定义 click() 函数，代替 domObj.onclick = function(){}
        this.click = function(fun) {
            domObj.onclick = fun;
        }
        // 定义 val() 函数，代替 domObj.value
        this.val = function(v) {
            if (v == undefined) {
                return domObj.value;
            } else {
                domObj.value = v;
            }
        }
    
        /**
         静态方法，发送 ajax 请求
            type 请求方式
            url 请求的 URL
            data 请求时提交的数据
            async 请求是否为异步请求
         */
        jQuery.ajax = function(jsonArgs) {
            var xhr = new XMLHttpRequest();
            xhr.onreadystatechange = function() {
                if (this.readyState === 4) {
                    if (this.states === 200) {
                        // 假设服务器返回的都是 json 格式的字符串
                        var jsonObj = JSON.parse(this.responseText);
                        // 调用函数
                        jsonArgs.success(jsonObj);
                    }
                }
            }
    
            if (jsonArgs.type.toUpperCase() === "POST") {
                xhr.open("POST", jsonArgs.url, jsonArgs.async);
                xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
                xhr.send(jsonArgs.data);
            }
    
            if (jsonArgs.type.toUpperCase() === "GET") {
                xhr.open("GET", jsonArgs.url + "?" + jsonArgs.data, jsonArgs.async);
                xhr.send();
            }
        }
    }
    $ = jQuery
    
    // 让静态方法 ajax 生效
    new jQuery()
    ```

- 使用以上 jQuery 库

  ```html
  <script type="text/javascript" src="jQuery 的路径"></script>
  
  <script type="text/javascript">
      // window.onload
  	$(function(){
          $("#btn1").click(function(){
              $.ajax({
                  type : "",
                  url : "",
                  data : "username=" + $("#username").val(),
                  async : true | false,
                  success : function(json){
                      $("#div1").html(json.username);
                  }
              })
          })
      })
  </script>
  ```







# AJAX 实现省市联动

- 什么是省市联动？
  - 在网页上，选择对应的省份之后，动态的关联出该省份对应的市。选择对应的市后，动态的关联出对应的区

- 数据库表设计

  ```
  t_area（区域表）
  id(PK-自增)	code	name	pcode
  -------------------------------------
  1			 001	 北京市	null
  2			 002	 上海市	null
  3			 003	 朝阳区	001
  4			 004	 海淀区	001
  5			 005	 闵行区	002
  6			 006	 徐汇区	002
  ```







# AJAX 跨域问题