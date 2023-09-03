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







# Servlet 接口中的方法

- init

  初始化方法，只执行一次

  初始化的一些代码可以放在该方法中

- service

  处理用户请求的核心方法

- destroy

  销毁 Servlet 对象前会调用一次，只执行一次

  释放资源的代码可以写在该方法中







# GenericServlet

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

- 对于一个 webapp 来说，所有 Servlet 对象共享一个 ServletContext 对象

- ServletContext 对象在服务器启动时创建，关闭时销毁。ServletContext 对象是应用级对象

  - 什么时候向 ServletContext 这个应用域当中绑定数据？
    - 第一：所有用户共享的数据
    - 第二：共享的数据量很小
    - 第三：共享的数据很少的修改操作
    - 向应用域中绑定数据，就相当于把数据放到了缓存（Cache）中，用户访问时直接从缓存中取，减少 IO 的操作，大大提升系统的性能
  - 见过的缓存技术：
    - 字符串常量池
    - 整数型常量池 [-128~127]
    - 数据库连接池（提前创建好 N 个连接对象，并放到集合中，使用连接对象时，直接从缓存中取，省去了连接对象的创建过程）
    - 线程池（提前创建好 N 个线程对象，存储到集合中，用户请求时，直接从线程池中获取线程对象）】
    - 后期还会学习更多的缓存技术：redis、mongoDB...

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
    ```
    
  - ```java
    // 通过 ServletContext 对象可以记录日志
    public void log(String message);
    public void log(String message, Throwable t);
    
    /*
    	日志信息记录到哪里？
    	localhost.date.log
    	
    	Tomcat服务器的logs目录下有哪些文件
    	catalina.date.log 服务器端的java程序运行的控制台信息
    	localhost.date.log ServletContext对象的log方法记录的日志信息
    	localhost_access_log.date.txt 访问日志
    */
    ```
    
  - 操作域的方法
    
    ```java
    //存（向ServletContext应用域中存数据）
    public void setAttribute(String name, Object value); // map.put(k, v)
    
    //取（从ServletContext应用域取数据）
    public Object getAttribute(String name); // Object v = map.get(k)
    
    //删（删除ServletContext应用域中的数据）
    public void removeAttribute(String name); // map.remove(k);
    ```







# HTTP 协议

- 什么是协议？

  - 协议是某些人或某些组织提前制定好的一套规范，大家都按照这个规范来，做到沟通无障碍

- 什么是 HTTP 协议？

  - HTTP 协议：W3C 制定的一种超文本传输协议（通信协议）

  - 这种协议游走在 B 和 S 之间，B --> S 或 S --> B 都需要遵循 HTTP 协议。这样 B 和 S 才能解耦合

    - 什么是解耦合？

      B 和 S 互相不依赖

- HTTP 协议包括

  - 请求协议
  - 响应协议

- 请求协议（B --> S）

  - 请求行
    - 请求方式（7种）
      - get（常用）
      - post（常用）
      - delete
      - put
      - head
      - options
      - trace
    - URI
    - HTTP 协议版本号

  - 请求头
    - 请求的主机
    - 主机的端口
    - 浏览器信息
    - 平台信息
    - cookie等信息
    - ...

  - 空白行
    - 用来区分“请求头”和“请求体”
  - 请求体
    - 向服务器发送的具体数据

- 响应协议 (S --> B)

  - 状态行
    - 第一部分：协议版本号（HTTP/1.1）
    - 第二部分：状态码
      - 200 表示请求响应成功，正常结束
      - 404 表示访问资源不存在
      - 405 表示前端发送的请求方式和后端请求的处理方式不一致
      - 500 表示服务器端的程序出现异常
      - 以 4 开始一般是浏览器端的错误
      - 以 5 开始一般是服务器端的错误
    - 第三部分：状态的描述信息
      - ok 正常成功结束
      - not found 资源找不到
  - 响应头
    - 响应的内容类型
    - 响应的内容长度
    - 响应的时间
    - ...
  - 空白行
    - 用来区分“响应头”和“响应体”
  - 响应体
    - 响应的正文，这些内容是一些字符串，这个字符串被浏览器渲染，解释并执行，最终展示出效果

- GET 请求和 POST 请求的区别
  - get 请求发送数据时，数据会在挂在 URI 后边，发送的数据会显示在浏览器地址栏（get 请求在请求行上发送数据）
  - post 请求发送数据时，在请求体当中发送数据，不会显示在浏览器地址栏
  - get 请求只能发送普通的字符串
  - get 请求无法发送大数据量
  - post 请求可以发送任何类型的数据
  - post 请求可以发送大数据量
  - get 请求在 W3C中是这样说的：get 请求比较适合从服务器端获取数据
  - post 请求在 W3C中是这样说的：post 请求比较适合向服务器端传送数据
  - get 请求是安全的，因为 get 请求只是为了从服务器端获取数据
  - post 请求是危险的，因为 post 请求向服务器端提交数据，如果这些数据通过后门的方式进入到服务器，是很危险的。所以一般在拦截请求时，拦截 post 请求
  - get 请求支持缓存
    - 任何一个 get 请求的响应结果都会被浏览器缓存
    - 发送 get 请求后，会先从本地缓存查找，找不到再从服务器获取，这种方法提高了用户体验
  - post 请求不支持缓存
  
- 发送的数据格式相同：
  - name=value&name=value&name=value...







# HttpServlet

- HttpServlet 类是专门为 HTTP 协议准备的，比 GenericServlet 更适合 HTTP 协议下的开发
- HttpServlet 在哪个包
  - jakarta.servlet.http
- http 包下的接口和类
  - jarkarta.servlet.http.HttpServlet（HTTP 协议专用的 Servlet 类，抽象类）
  - jarkarta.servlet.http.HttpServletRequest（HTTP 协议专用的请求对象）
  - jarkarta.servlet.http.HttpServletResponse（HTTP 协议专用的响应对象）

- 避免 405 错误
  - 前后端的请求方式要一致
    - 前端发送 get 请求时，就重写 doGet 方法
    - 前端发送 post 请求时，就重写 doPost 方法
    - ...

- 继承 HttpServlet 重写 service
  - 可以重写，但是由于覆盖了 service，享受不到 405 错误







# 最终的 Servlet 类开发

- 第一步：编写一个 Servlet 类，直接继承 HttpServlet
- 第二步：重写 doGet 或 doPost 方法
- 第三步：将 Servlet 类配置到 web.xml 中
- 第四步：准备前端的页面（form 表单），form 表单中指定请求路径







# 关于一个 web 站点的欢迎页面

- 在访问一个 webapp 时，如果没有指定资源路径，默认会访问欢迎页面

- 怎样设置欢迎页面？

  - 第一步：在 webapp 根目录下创建一个 html 文件

  - 第二步：配置 web.xml 文件

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
             version="4.0">
    
        <welcome-file-list>
            <welcome-file>xxx.html</welcome-file>
        </welcome-file-list>
    
    </web-app>
    ```

  - 第三步：启动服务器，访问 http://localhost:8080/项目名

- 一个 webapp 可以有多个欢迎页面，越靠上优先级越高

- 当文件名为 index.html 时，不需要在 web.xml 中配置欢迎页面

  - Tomcat 已经提前配置好了
  - 配置欢迎页面的两个地方：
    - webapp 内部的 web.xml 文件中（局部配置）
    - CATALINA_HOME/cof/web.xml 文件中（全局配置）
    - 注意：局部优先（就近原则）







# HttpServletRequest 接口

- HttpServletRequest 对象由 Tomcat 服务器负责创建，封装了 HTTP 的请求协议

- 面向 HttpServletRequest 接口编程，调用方法就可以获取请求的信息

- 常用的方法

  - 获取前端浏览器用户提交的数据

    - ```java
      String getParameter(String name); // 获取value数组中的第一个元素。最常用
      Map<String, String[]> getParameterMap(); // 获取Map
      Enumeration<String> getParameterNames(); // 获取Map中的所有key
      String[] getParameterValues(String name); // 根据key获取value
      ```

    - 前端 form 提交数据后，采用什么数据结构存储这些数据？

      - name=value&name=value&name=value...

      - Map 集合来存储

        ```java
        Map<String, String>
            这种想法是错误的
            如果采用以上的数据结构存储，会发现key重复时value覆盖
            
        Map<String, String[]>
            key存储String
            value存储String[]
            这样就可以避免value被覆盖
        ```

- request 对象又称为“请求域”对象

  - 请求域对象比应用域对象的范围小很多。生命周期很短

  - 请求域只在一次请求内有效

  - 请求域也有三个方法

    ```java
    void setAttribute(String name, Object obj); // 向域中绑定数据
    Object getAttribute(String name); // 从域中根据name获取数据
    void removeAttribute(String name); // 将域中绑定的数据移除
    ```

  - 跳转（一个 Servlet 中访问另一个 Servlet 的 request 对象）

    - 转发（一次请求）

      ```java
      // 第一步：获取请求转发器对象
      RequestDispatcher dispatcher = request.getRequestDispatcher("/另一个Servlet的路径");
      // 第二步：调用转发器的forward方法完成跳转/转发
      dispatcher.forward(request, response);
      
      // 第一步和第二步结合
      request.getRequestDispatcher("/另一个Servlet的路径").forward(request, response);
      ```

  - 两个 Servlet 怎样共享数据？

    - 将数据放到 ServletContext 应用域中，但是占用资源太多，不建议使用
    - 放到 request 域当中，然后 AServlet 转发给 BServlet
      - 注意：转发的路径不加项目名

  - request 对象中容易混淆的两种方法

    ```java
    // uri?username=xxx&userpwd=123
    String username = request.getParameter("username");
    
    //之前一定执行过：reqeust.setAttribute("name", new Object())
    Object obj = request.getAttribute("name");
    
    /*
    	以上两方法的区别
    	第一个方法：获取的是用户在浏览器上提交的数据
    	第二个方法：获取的是请求域中绑定的数据
    */
    ```

- HttpServletRequest 常用的其他方法

  ```java
  // 获取客户端的IP地址
  String remoteAddr = request.getRemoteAddr();
  
  // 设置请求体的字符集，解决Tomcat 9之前POST的乱码问题
  // Tomcat 10之后，request请求体当中的字符集默认就是UTF-8，
  request.setCharacterEncoding("UTF-8");
  
  // GET乱码怎么解决？
  // 修改CATALINI_HOME/conf/server.xml
  <Connector URIEncoding="UTF-8" />
  // Tomcat 8以后,URIEncoding默认为UTF-8
      
  // 获取应用根路径
  String contextPath = request.getContextPath();
  
  // 获取请求方式
  String method = request.getMethod();
  
  // 获取请求的URI
  String requestURI = request.getRequestURI(); // 带项目名
  
  // 获取Servlet Path
  String servletPath = request.getServletPath(); // 不带项目名
  ```







# 使用纯 Servlet 做一个单表的 CRUD 操作

- 使用纯粹的 Servlet 完成单表【对部门的】增删改查操作（B/S 结构）

- 实现步骤

  - 第一步：准备一张数据库表（sql 脚本）

    ```sql
    # 部门表
    set character_set_client = 'utf8';
    drop table if exists dept;
    create table dept (
    	deptno int primary key,
    	dname varchar(255),
    	loc varchar(255)
    );
    insert into dept(deptno, dname, loc) values(10, '销售部', '北京');
    insert into dept(deptno, dname, loc) values(20, '研发部', '上海');
    insert into dept(deptno, dname, loc) values(30, '技术部', '广州');
    insert into dept(deptno, dname, loc) values(40, '媒体部', '深圳');
    commit;
    select * from dept;
    ```

  - 第二步：准备一套 HTML 页面（项目原型）

    - 欢迎页面：index.html
    - 列表页面：list.html
    - 新增页面：add.html
    - 修改页面：edit.html
    - 详情页面：detail.html

  - 第三步：分析系统功能

    - 什么叫做一个功能？
      - 只要这个操作连接了数据库，就表示一个独立的功能
    - 包括哪些功能
      - 查看部门列表
      - 查看部门详细信息
      - 删除部门
      - 新增部门
      - 跳转到修改页面
      - 修改部门

  - 第四步：在 IDEA 当中搭建开发环境

    - 创建一个 webapp

    - 向 webapp 中添加连接数据库的 jar 包（mysql 驱动）

      - 在 WEB-INF 目录下新建一个 lib，将 mysql 的 jar 包拷贝进来

    - JDBC 的工具类

      ```java
      public class DBUtil {
          private static ResourceBundle bundle = ResourceBundle.getBundle("resources.jdbc");
          private static String driver = bundle.getString("driver");
          private static String url = bundle.getString("url");
          private static String user = bundle.getString("user");
          private static String password = bundle.getString("password");
      
          static {
              try {
                  Class.forName(driver);
              } catch (ClassNotFoundException e) {
                  e.printStackTrace();
              }
          }
      
          /**
           * 获取数据库连接对象
           * @return conn 连接对象
           * @throws SQLException
           */
          public static Connection getConnection() throws SQLException {
              // 获取连接
              Connection conn = DriverManager.getConnection(url, user, password);
              return conn;
          }
      
          /**
           * 释放资源
           * @param conn 连接对象
           * @param ps 数据库操作对象
           * @param rs 结果集对象
           */
          public static void close(Connection conn, Statement ps, ResultSet rs) {
              if (conn != null) {
                  try {
                      conn.close();
                  } catch (SQLException e) {
                      e.printStackTrace();
                  }
              }
              if (ps != null) {
                  try {
                      conn.close();
                  } catch (SQLException e) {
                      e.printStackTrace();
                  }
              }
              if (rs != null) {
                  try {
                      conn.close();
                  } catch (SQLException e) {
                      e.printStackTrace();
                  }
              }
          }
      }
      ```
    
  - 第五步：实现第一个功能——查看部门列表
  
    - 一：先修改前端页面的超链接
  
      ```html
      <a href="/oa/dept/list">查看部门列表</a>
      ```
  
    - 二：编写 web.xml 文件

      ```xml
      <servlet>
          <servlet-name>list</servlet-name>
          <servlet-class>com.shameyang.oa.web.action.DeptListServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>list</servlet-name>
          <url-pattern>/dept/list</url-pattern>
      </servlet-mapping>
      ```
  
    - 三：编写 DeptListServlet 类继承 HttpServlet，然后重写 doGet 方法
  
      ```java
      public class DeptListServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp)
                  throws ServletException, IOException {
              
          }
      }
      ```
      
    - 四：在 DeptListServlet 类的 doGet 方法中连接数据库，查询所有的部门，动态展示部门列表页面
    
      ```java
      int i = 0;
      while (rs.next()) {
      	String no = rs.getString("no");
      	String name = rs.getString("name");
      	String loc = rs.getString("loc");
      
      	out.print("  <tr>");
      	out.print("    <td>" + (++i) + "</td>");
      	out.print("    <td>" + no + "</td>");
      	out.print("    <td>" + name + "</td>");
      	out.print("    <td>" + loc + "</td>");
      	out.print("    <td>");
      	out.print("      <a href=\"\">删除</a>");
      	out.print("      <a href=\"\">修改</a>");
      	out.print("      <a href=\"\">详情</a>");
      	out.print("    </td>");
      	out.print("  </tr>");
      }
      ```
    
  - 第六步：实现功能——查看部门详情
  
    - 从前端往后端一步步实现，先找到用户点击的详情在哪里
  
      - 在后端 java 程序中找到
  
        ```java
        out.print("<a href='写一个路径'>详情</a>");
        ```
  
      - 该超链接的路径应该带项目名，我们也可以实现动态获取
  
        ```java
        out.print("<a href='" + contextPath + "/dept/detail?deptno=" + no + "'>详情</a>");
        ```
  
      - 上边为什么要用 ?deptno=
  
        根据 HTTP 协议规定，向服务器提交数据的格式：URI?name=value&name=value
  
    - 解决 404 问题，写 web.xml 文件
  
      ```xml
      <servlet>
          <servlet-name>detail</servlet-name>
          <servlet-class>com.shameyang.oa.web.action.DeptDetailServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>detail</servlet-name>
          <url-pattern>/dept/detail</url-pattern>
      </servlet-mapping>
      ```
  
    - 编写 DeptDetailServlet 类继承 HttpServlet，然后重写 doGet 方法
  
      ```java
      public class DeptDetailServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp)
                  throws ServletException, IOException {
              
          }
      }
      ```
  
    - doGet 方法中连接数据库，根据部门编号查询部门信息，动态展示部门详情页面
  
      ```java
      conn = DBUtil.getConnection();
      String sql = "select dname, loc from dept where deptno = ?";
      ps = conn.prepareStatement(sql);
      ps.setString(1, deptno);
      rs = ps.executeQuery();
      if (rs.next()) {
          String dname = rs.getString("dname");
          String loc = rs.getString("loc");
      
          out.print("  部门编号：" + deptno + "<br>");
          out.print("  部门名称：" + dname + "<br>");
          out.print("  部门位置：" + loc + "<br>");
      }
      ```
  
  - 第七步：删除部门功能
  
    - 从前端页面开始，用户点击删除按钮时，提示用户是否删除，防止误删
  
      ```html
      <a href="javascript:void(0)" onclick="del(10)">删除</a>
      <script type="text/javascript">
      	function del(dno) {
      		if (window.confirm("确定删除吗？")) {
      			document.location.href = "/oa/dept/delete?deptno=" + dno;
              }
          }
      </script>
      ```
  
    - 以上的前端代码要写到后端的 java 代码中
  
      - DeptListServlet 类的 doGet 方法中，使用 out.print() 方法，将以上代码输出到浏览器上
  
    - 编写 web.xml 文件
  
      ```xml
      <!--删除部门-->
      <servlet>
          <servlet-name>delete</servlet-name>
          <servlet-class>com.shameyang.oa.web.action.DeptDeleteServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>delete</servlet-name>
          <url-pattern>/dept/delete</url-pattern>
      </servlet-mapping>
      ```
  
    - 编写 DeptDeleteServlet 类继承 HttpServlet，然后重写 doGet 方法
  
      ```java
      public class DeptDeleteServlet extends HttpServlet {
          @Override
          protected void doGet(HttpServletRequest req, HttpServletResponse resp)
                  throws ServletException, IOException {
              // 根据部门编号，删除部门
          }
      }
      ```
  
    - 删除成功或失败的处理（这里我们采用转发，没有使用重定向）
  
      ```java
      // 判断删除成功还是失败
      if (count == 1) {
          // 删除成功则跳转到部门列表页面
          // 执行另一个Servlet，需要转发
          req.getRequestDispatcher("/dept/list").forward(req, resp);
      } else {
          // 删除失败
          req.getRequestDispatcher("/error.html").forward(req, resp);
      }
      ```
  
  - 第八步：新增部门功能
  
    - 获取 add 页面
  
      ```html
      out.print("<a href='" + contextPath + "/add.html'>新增部门</a>");
      ```
  
    - add.html
  
      ```html
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>新增部门</title>
      </head>
      <body>
          <h1>新增部门</h1>
          <hr>
          <form action="/oa/dept/save" method="post">
              部门编号：<input type="text" name="deptno"/><br>
              部门名称：<input type="text" name="dname"/><br>
              部门位置：<input type="text" name="loc"/><br>
              <input type="submit" value="保存"/>
          </form>
      </body>
      </html>
      ```
  
    - 编写 web.xml
  
      ```xml
      <!--新增部门-->
      <servlet>
          <servlet-name>add</servlet-name>
          <servlet-class>com.shameyang.oa.web.action.DeptDeleteServlet</servlet-class>
      </servlet>
      <servlet-mapping>
          <servlet-name>add</servlet-name>
          <url-pattern>/dept/save</url-pattern>
      </servlet-mapping>
      ```
  
    - 编写 DeptSaveServlet 类继承 HttpServlet，然后重写 doPost 方法
  
      ```java
      public class DeptSaveServlet extends HttpServlet {
          @Override
          protected void doPost(HttpServletRequest req, HttpServletResponse resp)
                  throws ServletException, IOException {
              // 获取部门信息
          }
      }
      ```
  
    - 解决 405 问题
  
      - 由于 DeptSaveServlet 中重写的是 doPost 方法，转发到 /dept/list 后，DeptListServlet 类中我们重写的是 doGet 方法，所以会导致 405 问题
      - 我们可以在 DeptListServlet 类中重写 doPost 方法，然后调用 doGet 方法来解决
  
  - 第九步：修改部门信息功能
  
    - 跳转到修改信息页面
    - 点击修改按钮完成修改，返回部门列表页面







# web 应用中资源的跳转

- 一个 web 应用中，可以用过两种方式完成资源的跳转：

  - 转发
  - 重定向

- 转发和重定向的区别？

  - 代码上的区别

    - 转发

      ```java
      request.getRequestDispatcher("/xxx").forward(request, response);
      ```

    - 重定向

      ```java
      response.sendRedirect(request.getContextPath() + "/xxx");
      ```

  - 形式上的区别

    - 转发是一次请求，请求结束后地址栏上的地址不变
    - 重定向是两次请求，最终显示在地址栏上的地址为重定向的地址

  - 转发和重定向本质上的区别

    - 转发：是由 WEB 服务器控制的。A 资源跳转到 B 资源，由服务器内部完成
    - 重定向：是浏览器完成的。具体跳转到哪个资源，由浏览器决定

  - 转发

    <img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E8%BD%AC%E5%8F%91.png"  />

  - 重定向

    ![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E9%87%8D%E5%AE%9A%E5%90%91.png)

- 转发与重定向的选择
  - 如果在一个 Servlet 当中向 request 域绑定了数据，希望从另一个 Servlet 中把里边的数据取出来时，使用转发机制
  - 剩下所有的请求均使用重定向







# Servlet 注解式开发

- 我们之前的 web.xml 文件中进行信息配置，效率较低，因此可以将配置信息直接写到 Java 类中

- Servlet 3.0之后，推出了各种 Servlet 基于注解式开发

- 并不是说有了注解之后，web.xml 文件就不需要了：

  - 一些需要变化的信息，要写到 web.xml 文件中。一般都是 注解+配置文件 的开发模式
  - 一些不会经常变化修改的配置建议使用注解

- 我们的第一个注解：

  - ```
    jakarta.servlet.annotation.WebServlet
    ```

  - 在类上使用 @WebServlet

  - @WebServlet 中常用的属性

    - name 属性：用来指定 Servlet 的名字。等同于：`<servlet-name>`
    - urlPatterns 属性：用来指定 Servlet 的映射路径。可以指定多个字符串，等同于：`<url-pattern>`
    - value 属性：与 urlPatterns 一样，都是用来指定 Servlet 映射路径的。
      - 当注解的属性名为 value 时，属性名可以省略
    - loadOnStartup 属性：用来指定在服务器启动阶段是否加载该 Servlet。等同于：`<load-on-startup>`
    - initParams 属性：用来指定初始化参数。等同于：`<init-param>`
    - 注意：属性是一个数组，如果数组中只有一个元素，大括号可以省略

- 注解对象的使用格式：
  - @注解名称(属性名=属性值, 属性名=属性值...)