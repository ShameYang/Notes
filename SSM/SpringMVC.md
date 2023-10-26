# 一、SpringMVC 简介

## 1.1 什么是 MVC

MVC 是一种软件架构的思想，将软件划分为模型、视图、控制器

M：Model，模型层，指工程中的 JavaBean，负责处理数据

- JavaBean 分为两类：
  - 实体类 Bean：负责存储业务数据
  - 业务处理 Bean：Servlet 或 Dao 对象，用于处理业务逻辑和数据访问

V：View，视图层，指工程中的 html 或 jsp 页面，负责与用户交互，展示数据

C：Controller，控制层，指工程中的 servlet，负责接收请求和响应浏览器



## 1.2 什么是 SpringMVC

SpringMVC 是 Spring 的一个模块，基于 MVC 开发模式的框架，用来优化控制层，专门用来做 web 开发



## 1.3 SpringMVC 框架的特点

① 轻量级，简单易学

② 高效 , 基于请求响应的 MVC 框架

③ 与 Spring 兼容性好，无缝结合

④ 约定优于配置

⑤ 功能强大：RESTful、数据验证、格式化、本地化、主题等

⑥ 简洁灵活