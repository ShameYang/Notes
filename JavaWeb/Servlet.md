# 系统架构

- C / S 架构
  - Client / Server（客户端 / 服务器端的交互形式）
  - 常见的系统：QQ、微信、支付宝...
  - 优点
    - 速度快（软件中大部分数据都集成到客户端）
    - 体验好
    - 界面炫酷（专门的语言实现，更灵活）
    - 服务器压力小（大部分数据集成到客户端，很少传输到服务器）
    - 安全（大量数据在客户端有缓存，而服务器只有一个，避免服务器造成的风险）
  - 缺点：升级麻烦，维护成本高



- B / S 架构

  - B / S 架构是特殊的 C / S 架构，Client 是固定不变的浏览器

  - Browser / Server（浏览器 / 服务器的交互形式）
  - 支持语言：HTML、CSS、JavaScript
  - 常见的系统：京东、淘宝、百度...
  - 优点：升级方便（只升级服务端代码即可），维护成本低（企业大多采用 B / S 架构）
  - 缺点：速度慢、体验不好、界面不炫酷







# B/S 结构的系统通信原理

- WEB 系统的访问过程
  - 打开浏览器
  - 找到地址栏
  - 输入一个合法的网址
  - 回车
  - 浏览器上展示响应的结果
- 关于域名
  - 浏览器上输入域名，回车之后，域名解析器会将域名解析出来一个具体的 IP 地址和端口号
- IP 地址
  - 同一个网络当中，IP 地址是唯一的，用来标识计算机
- 端口号
  - 一个端口代表一个软件
  - 同一台计算机上，端口号唯一
- WEB 系统的通信步骤
  - 第一步：用户输入网址 URL
  - 第二步：域名解析器进行域名解析
  - 第三步：浏览器在网络中根据 IP 地址搜素主机
  - 第四步：根据端口号，在主机上定位相应软件
  - 第五步：得到资源名
  - 第六步：将文件响应到浏览器上
  - 第七步：浏览器接收到来自服务器的代码
  - 第八步：浏览器渲染，执行代码，展示效果







# WEB 服务器软件

- WEB 服务器有哪些
  - Tomcat（WEB 服务器）
  - jetty（WEB 服务器）
  - JBOSS（应用服务器）
  - WebLogic（应用服务器）
  - WebSphere（应用服务器）

- WEB 服务器和应用服务器的关系
  - WEB 服务器只实现了 JavaEE 中的 Sevlet + JSP 两个核心的规范
  - 应用服务器实现了 JavaEE 的所有规范







# 配置 Tomcat 服务器

- 环境变量
  - JAVA_HOME=JDK 的根
  - CATALINA_HOME=Tomcat 服务器的根
  - PATH=%JAVA_HOME%\bin;%CATALINA_HOME%\bin
- 启动 Tomcat：startup.bat
- 关闭 Tomcat：shutdown.bat（注意一定要带.bat，否则会关机！！！这里我们可以重命名为 stop，避免手误）
- 测试 Tomcat 有没有成功启动
  - 打开浏览器，输入 URL
    - http://ip地址:端口号
    - ip 地址为 127.0.0.1 或 localhost
    - 端口号为 8080







# 实现一个最基本的 web 应用

1. 找到 CATALINA_HOME\webapps 目录
2. 在该目录下新建一个子目录，例如 test
3. 在 test 目录下新建一个 index.html 文件（编写其中的内容）
4. 启动 Tomcat 服务器
5. 打开浏览器，地址栏输入 http://localhost:8080/test/index.html







# B/S结构系统的角色和协议

- 角色
  - 浏览器软件开发团队（谷歌、火狐...）
  - WebServer 的开发团队（Tomcat、Jetty、Weblogic...）
  - DB Server 的开发团队（Oracle、MySQL...）
  - Webapp 的开发团队（JavaWeb 程序员）
- 规范、协议
  - Webapp 的开发团队和 WebServer 的开发团队之间的规范：JavaEE 规范之一 Servlet
    - Servlet 规范的作用：WebServer 和 Webapp 解耦合
  - Browser 和 WebServer 之间的传输协议：HTTP协议（超文本传输协议）
  - Webapp 开发团队和 DB Server 的开发团队之间的规范：JDBC 规范

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/BS%E7%BB%93%E6%9E%84%E7%B3%BB%E7%BB%9F%E7%9A%84%E8%A7%92%E8%89%B2%E5%92%8C%E5%8D%8F%E8%AE%AE.png)







# 模拟 Servlet 本质

- 充当 Sun 公司的角色，制定 Servlet 规范

  ```java
  package javax.servlet;
  
  /* 
  	我们现在充当的是 Sun 公司的角色，
  	制定 Servlet 规范
  */
  public interface Servlet {
      //一个专门提供服务的方法
      void service();
  }
  ```

  

- 充当 Tomcat 服务器的开发者

  ```java
  package org.apache;
  
  import java.util.ResourceBundle;
  import javax.servlet.Servlet;
  
  //充当 Tomcat 的开发者
  public class Tomcat {
      public static void main(String[] args) {
          System.out.println("Tomcat 服务器启动成功");
          //Tomcat 服务器应该通过用户的请求路径找对应的XXXServlet
          //对于 Tomcat 服务器来说，需要解析配置文件
          ResourceBundle bundle = ResourceBundle.getBundle("配置文件");
          String key = bundle.getString("请求路径");
          String value = bundle.getString("XXXServlet");
          //通过反射机制创建对象
          Class clazz = Class.forName(value);
          Object obj = clazz.newInstance(); // obj 的类型对于Tomcat服务器开发人员来说不知道
          
          //但是Tomcat服务器的开发者知道一定实现了Servlet接口
         	Servlet servlet = (Servlet)obj;
          servlet.service();
      }
  }
  ```

  

- 充当 Webapp 的开发者

  ```java
  package com.shameyang.servlet;
  
  import javax.servlet.Servlet;
  
  //充当的角色：Webapp 的开发者
  
  public class BankServlet implements Servlet {
      public void service() {
          System.out.println("BankServlet's service...");
      }
  }
  ```

  