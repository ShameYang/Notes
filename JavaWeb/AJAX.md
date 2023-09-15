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