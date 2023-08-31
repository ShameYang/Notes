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
  - Jetty（WEB 服务器）
  - JBOSS（应用服务器）
  - WebLogic（应用服务器）
  - WebSphere（应用服务器）

- WEB 服务器和应用服务器的关系
  - WEB 服务器只实现了 JavaEE 中的 Servlet + JSP 两个核心的规范
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







# 解决 Tomcat 服务器在 DOS 命令窗口的乱码问题（控制台乱码）

将 CATALINA_HOME/conf/logging.properties 中的内容修改如下：



java.util.logging.ConsoleHandler.encoding = GBK







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
          ResourceBundle bundle = ResourceBundle.getBundle("web.properties");
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




- 对于我们 JavaWeb 程序员来说，我们只需做两件事：
  - 编写一个类实现 Servlet 接口
  - 将编写的类配置到配置文件中，在配置文件中指定请求路径和类名的关系
  
  - 注意：
    - 配置文件名固定
    - 配置文件的存放路径固定
    - 文件名、文件路径都是 SUN 公司制定的 Servlet 规范中的明细
  
- 关于 Servlet
  - 遵循 Servlet 的 webapp，可以放在不同的 WEB 服务器中运行	
  - Servlet 包括：
    - 规范了哪些接口
    - 规范了哪些类
    - 规范了一个 web 应用中应该有哪些配置文件
    - 规范了一个 web 应用中配置文件的名字
    - 规范了一个 web 应用配置文件存放的路径
    - 规范了一个 web 应用配置文件的内容
    - 规范了一个合法有效的 web 应用的目录结构







# 开发第一个 Servlet

- 开发步骤

  - 第一步：在 webapps 目录下新建一个目录，例如 crm

    - crm 就是 webapp 的根

  - 第二步：在 crm 目录下，新建一个目录：WEB-INF

    - 该名字是 Servlet 规范中规定的，必须全部大写

  - 第三步：WEB-INF 目录下，新建一个目录：classes

    - 该名字也是规定的，必须全部小写
    - 该目录下存放的一定是 class 文件

  - 第四步：WEB-INF 目录下，新建一个目录：lib

    - 该目录非必须，但是需要使用第三方 jar 包时，需要放到该目录下

  - 第五步：WEB-INF 目录下，新建一个文件：web.xml

    - 该目录必须有，这个配置文件中描述了请求路径和 Servlet 类之间的对照关系

    - 这个文件最好直接从其他的 webapp 复制粘贴

      ```xml
      <?xml version="1.0" encoding="UTF-8"?>
      <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                            https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
        version="6.0"
        metadata-complete="true">
      
      
      </web-app>
      ```

  - 第六步：编写一个 Java 程序，必须实现 Servlet 接口

    - Servlet 接口不在 JDK 中（因为 Servlet 是 JavaEE 的）
    - Tomcat 服务器实现了 Servlet 规范，所以在 Tomcat 服务器中有该接口，CATALINA_HOME\lib 目录下有一个 servlet-api.jar，解压之后会看到 Servlet.class
    
  - 第七步：编译我们编写的 Java 程序
  
    - 重点：如何让程序编译通过？
  
      配置环境变量 CLASSPATH
  
      CLASSPATH=.;servlet-api.jar的路径
  
  - 第八步：将编译后的 class 文件拷贝到 WEB-INF\classes 目录下
  
  - 第九步：在 web.xml 中编写配置信息，关联请求路径和 Servlet 类名
  
    - 专用术语：在 web.xml 文件中注册 Servlet 类
  
    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                          https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
      version="6.0"
      metadata-complete="true">
        
    	<!--servlet描述信息-->
        <!--任意一个servlet都对应一个servlet-mapping-->
        <servlet>
            <servlet-name>aaaa</servlet-name>
            <!--必须是带有包名的全限定类名-->
            <servlet-class>com.shameyang.servlet.HelloServlet</servlet-class>
        </servlet>
        
        <!--servlet映射信息-->
        <servlet-mapping>
            <!--这个也是随便的，但是要和上面一样-->
            <servlet-name>aaaa</servlet-name>
            <!--这里需要一个路径，路径必须以 / 开始-->
            <url-pattern>/...</url-pattern>
        </servlet-mapping>
        
    </web-app>
    ```
  
  - 第十步：启动 Tomcat 服务器
  
  - 第十一步：浏览器上输入 url
  
    - http://127.0.0.1:8080/crm/配置文件中的路径
    - 非常重要：浏览器上的请求路径必须和 web.xml 中的 url-pattern 一致
    - 注意：浏览器上的请求路径与 web.xml 中的 url-pattern 的唯一区别：浏览器上的请求路径带项目名：/crm
  
  - 浏览器上要编写的路径太复杂时，可以使用超链接（html 页面必须放到 WEB-INF 目录外）



- 总结：

  - 合法的 webapp 目录结构

  ```
  webapproot
  	|------WEB_INF
  		|------classes(存放字节码)
  		|------lib(第三方jar包)
  		|------web.xml(注册Servlet)
  	|------html
  	|------css
  	|------javascript
  	|------img
  	...
  ```

  - 浏览器发送请求，到最终服务器调用 Servlet 中的方法，是怎样的过程？
    - 用户输入 URL，或者点击超链接
    - 然后 Tomcat 服务器收到请求，截取路径
    - Tomcat 服务器根据截取的路径找到项目根目录
    - Tomcat 服务器在 web.xml 文件中查找 url-pattern 路径对应的 Servlet 实现类
    - Tomcat 服务器通过反射机制，创建实现类的对象
    - Tomcat 服务器 调用该对象的 service 方法







# 关于 JavaEE 版本

- 目前最高版本为 JavaEE8
- JavaEE 被 Oracle 捐献给了 Apache，改名为 Jakarta EE
- JavaEE8 时类名：javax.servlet.Servlet
- JavaEE9 时类名：jakarta.servlet.Servlet







# 向浏览器响应一段 HTML 代码

```java
public void service(ServletRequest request, ServletResponse response) {
    response.setContentType("text/html");
    PrintWriter out = response.getWriter();
    out.print("<h1>Hello Servlet!</h1>");
}
```







# Servlet 中连接数据库

- 因为 Servlet 是 Java 程序，所以 Servlet 中可以编写 JDBC 代码连接数据库
- 在一个 webapp 中去连接数据库，需要将驱动 jar 包放到 WEB-INF/lib 目录下







# 集成开发环境开发 Servlet

- 使用 IDEA 开发 Servlet

  - 第一步：New Project

    - Empty Project 起名为：javaweb

  - 第二步：新建模块

    - File --> new --> Module

    - 这里新建的是一个 JavaSE 的模块（先不新建 Java Enterprise 模块）
    - 这个 Module 会被自动放在 javaweb 的 project 下边

  - 第三步：让 Module 变成 JavaEE 的模块

    - 在 Module 上右键：Add Framework Support

    - 选择 Web Application

    - 选择之后，IDEA 会自动生成一个符合 Servlet 规范的 webapp 目录结构

      这个 web 目录就代表 webapp 的根

  - 第四步（非必须）：删除生成的 index.jsp 文件

  - 第五步：导入 jar 包，编写 XXXServlet

    - 导入 jar 包：

      File --> Project Structure --> Module --> Dependencies --> + 号添加 jsp-api.jar 和 servlet-api.jar

    - 实现 Servlet 接口中的方法

  - 第六步：WEB-INF 目录下新建子目录 lib，将连接数据库的 jar 包放到里边

  - 第七步：在 web.xml 中完成 XXXServlet 类的注册

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
        <servlet>
            <servlet-name>xxxServlet</servlet-name>
            <servlet-class>com.shameyang.javaweb.servlet.XXXServlet</servlet-class>
        </servlet>
        
        <servlet-mapping>
            <servlet-name>xxxServlet</servlet-name>
            <url-pattern>/...</url-pattern>
        </servlet-mapping>
    </web-app>
    ```

  - 第八步：给一个 html 页面，在页面中编写一个超链接，用户点击这个超链接后，发送请求，Tomcat 执行后台的 XXXServlet

    - 该 html 只能放在 WEB-INF 目录外

    ```html
    <!doctype html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport"
              content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0">
        <meta http-equiv="X-UA-Compatible" content="ie=edge">
        <title>xxx page</title>
    </head>
    <body>
        <!--这里的 pname 是项目名，无法动态获取，先写死-->
        <a href="/pname/...">xxx list</a>
    </body>
    </html>
    ```

  - 第九步：IDEA 连接 Tomcat 服务器

    - 关联的过程中，将 webapp 部署到 Tomcat 服务器中
    - 右上角小锤子右边：Add Configuration
    - 添加 Tomcat Server --> Local
    - Deployment（用来部署 webapp），添加 Artifact，修改 Application context 修改为 项目名

  - 第十步：启动 Tomcat 服务器

  - 第十一步：打开浏览器，地址栏输入 http://127.0.0.1/pname/index.html







# Servlet 对象的生命周期

- Servlet 对象的生命周期由 Tomcat 服务器负责
  - Tomcat 服务器又称为：WEB 容器（WEB Container）
  - 又可以理解为 WEB 容器管理 Servlet 对象的死活
  - 我们自己创建的 Servlet 对象不受 WEB 容器管理
  - WEB 容器创建的 Servlet 对象都会被放到一个集合中（HashMap）

- Servlet 对象创建：用户请求时

  - Tomcat 服务器启动时，Servlet 对象不会实例化

  - 在实现 Servlet 接口类中创建一个无参构造方法，启动时我们会发现该方法不执行

  - 启动时做了什么？

    Tomcat 服务器会把 web.xml 文件中的请求路径和类名放到 Map 集合中

- 怎样在服务器启动时，创建 Servlet 对象

  - 在 web.xml 文件 servlet 标签中添加 `<load-on-startup>整数</load-on-startup>` 数字越小，优先级越高

- Servlet 对象是单例的
  - 单实例，但是 Servlet 类不符合单例模式，所以称之为假单例（真单例模式，构造方法是私有化的）
  - init 方法只在第一次用户发送请求时执行
  - destroy 方法只在服务器关闭时调用一次







# 关于 Servlet 接口中的方法

- init

  初始化方法，只执行一次

  初始化的一些代码可以放在该方法中

- service

  处理用户请求的核心方法

- destroy

  销毁 Servlet 对象前会调用一次，只执行一次

  释放资源的代码可以写在该方法中







# 适配器模式改造 Servlet

> 由于 Servlet 接口中许多方法不常用，所以我们可以通过适配器模式，只重写 service 方法
>
> jakarta.servlet 包下有一个 GenericServlet 类， 我们直接继承就可以

- 编写一个 GenericServlet 类，这个类是抽象类，其中有一个抽象方法 service

  - GenericServlet 类实现 Servlet 接口

  - 该类是一个适配器

    - 什么是适配器？

      以手机为例，直接充到 220V 电源就废了，所以需要通过充电器，充电器就是一个适配器

  - 之后编写的 Servlet 类都继承 GenericServlet 类，重写 service 方法即可

- 改造 GenericServlet 类

  - 为了方便子类的service方法调用 ServletConfig 对象，将其设置为成员变量

    以下代码是模仿源码写的，jakarta.servlet 包下有一个 GenericServlet 类

    ```java
    package com.shameyang.javaweb.servlet;
    
    import jakarta.servlet.*;
    
    import java.io.IOException;
    
    /**
     * @author ShameYang
     * @date 2023/8/31 11:38
     * @description 适配器模式改造 Servlet
     */
    public abstract class GenericServlet implements Servlet {
        // ServletConfig成员变量，方便子类的 service 方法中调用
        private ServletConfig servletConfig;
    
        /**
         * 将该init方法设置为final，防止子类重写，改变servletConfig
         * 再调用一个可以继承的init方法，便于子类重写
         */
        @Override
        public final void init(ServletConfig servletConfig) throws ServletException {
            this.servletConfig = servletConfig;
            this.init();
        }
    
        /**
         * 提供子类重写的init方法
         */
        public void init() {
    
        }
    
        @Override
        public ServletConfig getServletConfig() {
            return servletConfig;
        }
    
        /**
         * 将核心方法设置为抽象方法，方便子类调用
         */
        @Override
        public abstract void service(ServletRequest servletRequest, ServletResponse servletResponse)
                throws ServletException, IOException;
    
        @Override
        public String getServletInfo() {
            return null;
        }
    
        @Override
        public void destroy() {
    
        }
    }
    ```

    ```java
    package com.shameyang.javaweb.servlet;
    
    import jakarta.servlet.ServletConfig;
    import jakarta.servlet.ServletException;
    import jakarta.servlet.ServletRequest;
    import jakarta.servlet.ServletResponse;
    
    import java.io.IOException;
    
    /**
     * @author ShameYang
     * @date 2023/8/31 11:54
     * @description GenericServlet的子类
     */
    public class XXXServlet extends GenericServlet {
        @Override
        public void service(ServletRequest servletRequest, ServletResponse servletResponse)
                throws ServletException, IOException {
            // 在XXXServlet中使用ServletConfig对象
            ServletConfig servletConfig = this.getServletConfig();
            System.out.println("查看能否调用:" + servletConfig);
        }
    }
    ```







# ServletConfig

> ServletConfig 接口中的方法都被 GenericServlet 重写了，我们也可以继承 GenericServlet 调用方法

- 什么是 ServletConfig？

  - Servlet 对象的配置信息对象
  - ServletConfig 对象中封装了`<servlet></servlet>`中的配置信息（web.xml 中 servlet的配置信息）

- 一个Servlet 对象对应一个 ServletConfig 对象，默认情况下，在用户发送第一次请求时创建

- ServletConfig 对象由 Tomcat 服务器创建

- Tomcat 服务器调用 Servlet 对象的 init 方法时需要传一个 ServletConfig 对象的参数

- ServletConfig 接口的实现类是 Tomcat 服务器给实现的

- ServletConfig 接口的常用方法

  ```java
  public String getInitParameter(String name); // 通过初始化参数的name获取value
  
  public Enumeration<String> getInitParameterNames(); // 获取所有的初始化参数的name
  
  public ServletContext getServletContext(); // 获取 ServletContext 对象
  
  public String getServletName(); // 获取Servlet的name
  ```







# ServletContext

- ServletContext 是一个接口，Tomcat 实现了该接口

- 对于一个 webapp 来说，所有 Servlet 对象 共享一个 ServletContext 对象

- ServletContext 对象在服务器启动时创建，关闭时销毁。ServletContext 对象是应用级对象

- ServletContext 对象被称为 Servlet 上下文对象

- 一个 ServletContext 对象通常对应一个 web.xml 文件

- ServletContext 接口的常用方法

  - ```java
    public String getInitParameter(String name);
    
    public Enumeration<String> getInitParameterNames();
    ```

    ```java
    <!--以上两个方法是 ServletContext 对象的方法，获取如下的配置信息-->
    	<context-param>
        	<param-name>pageSize</param-name>
            <param-value>10</param-value>
        </context-param>
        
        <context-param>
        	<param-name>startIndex</param-name>
            <param-value>0</param-value>
        </context-param>
    <!--
    	以上的配置信息属于应用级的配置信息，一般一个项目中共享的配置信息会放到以上标签中
    	如果只是想给某一个servlet作为参考，配置到servlet标签中即可
    -->
    ```

  - ```java
    public String getContextPath(); // 获取应用的根路径
    
    public String getRealPath(String path); // 获取文件的绝对路径
    
    // 通过 ServletContext 对象可以记录日志
    public void log(String message);
    public void log(String message, Throwable t);