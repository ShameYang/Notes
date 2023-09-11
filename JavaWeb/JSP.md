# JSP 概述

- JSP 实际上就是一个 Servlet
  - index.jsp 访问时，会自动翻译生成 index_jsp.java，然后自动编译生成 index_jsp.class
  - index_jsp 继承了 HttpJspBase，HttpJspBase 继承了 HttpServlet，所以 JSP 实际上就是一个 Servlet 类
  - Jsp 和 Servlet 的生命周期一样，没有区别
  - Jsp 也是单例的（假单例）
- JSP 是什么
  - JavaServer Pages，基于 Java 实现的服务器端的页面
  - JSP 就是一个 Java 程序（本质上是一个 Servlet）
  - JSP 也是 JavaEE 的十三个子规范之一
  - JSP 是一种规范，每个 WEB 容器/ WEB 服务器都会遵循该规范，按照该规范“翻译”
  - 每个 WEB 容器/ WEB服务器都会内置 JSP 翻译引擎
- JSP 与 Servlet 的区别？
  - 职责不同
    - Servlet 的职责是：收集数据（处理业务）
    - JSP 的职责是：展示数据







# JSP 基础语法

- 解决响应时中文乱码问题：

  ```jsp
  <%@page contentType="text/html;charset=UTF-8" %>
  ```

- JSP 代码中编写 Java 代码：

  - <% Java code %>

    - Java 代码会被写到 service 方法内部
    - 注意：此处的 Java code 相当于 Java 的“方法体”内，需要符合语法规则

  - <%! Java code %>

    - Java 代码会被写到类体的位置
    - 这个语法很少使用，存在线程安全问题

  - 输出语句 <%  %>

    - ```jsp
      <% 
      	String name = "tom";
      	out.write("name = " + name);
      %>
      ```

    - 以上代码中的 out 是 JSP 九大内置对象之一，可以直接使用，必须只能在 service 方法内使用

    - 如果输出的内容中没有 Java code，直接在 JSP 中编写即可

  - 输出语句 <%= %>

    - 这个符号会被翻译成 out.print();

    - 输出变量时，使用该符号比较方便

      ```jsp
      <% String name = "tom";%>
      <%= name %>
      ```

- JSP 的注释

  ```jsp
  <%-- 这是JSP的专业注释，不会被翻译到 Java 源代码中 --%>
  ```



## JSP 语法总结

- JSP 中直接编写普通字符串
  - 翻译到 service 方法的 out.writer("这里");
- <% %>
  - 翻译到 service 方法体内部
- <%! %>
  - 翻译到 service 方法外
- <%= %>
  - 翻译到 service 方法内，翻译为：out.print();
- <%@page contentType="text/html;charset=UTF-8" %>
  - page 指令，通过 contentType 属性设置响应的类型







# Servlet + JSP 改造 oa 项目

> Servlet 收集数据（处理业务），JSP 展示数据

- 将之前准备的 html 文件改为 jsp 文件，完成所有页面的正常流转（修改超链接的请求路径）

  - 例如 list.jsp

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" %>
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>欢迎使用OA系统</title>
    </head>
    <body>
    <%--<a href="/oa/list.jsp">查看部门列表</a>--%>    
    <a href="<%= request.getContextPath() %>/list.jsp">查看部门列表</a>
    </body>
    </html>
    ```

  - 上边`<%= request.getContextPath() %>`是动态获取应用的根路径


- 修改超链接的请求路径，映射到对应的 Servlet 类

  - 例如，修改 index.jsp

    ```jsp
    <%@ page contentType="text/html;charset=UTF-8" %>
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <title>欢迎使用OA系统</title>
    </head>
    <body>
    <%--<a href="/oa/list.jsp">查看部门列表</a>--%>
    <%--<a href="<%= request.getContextPath() %>/list.jsp">查看部门列表</a>--%>
    <a href="<%= request.getContextPath() %>/dept/list">查看部门列表</a>
    </body>
    </html>
    ```

  - 映射到对应的 Servlet 类，执行相应的操作

    ```java
    @WebServlet("/dept/list")
    public class DeptServlet extends HttpServlet {
        @Override
        protected void service(HttpServletRequest request, HttpServletResponse response)
                throws ServletException, IOException {
            String servletPath = request.getServletPath();
            if (servletPath.equals("/dept/list")) {
                doList(request, response);
            }
        }
    
        /**
         * 连接数据库，查询所有的部门信息，将部门信息收集好，然后跳转到 JSP 页面展示
         * @param request
         * @param response
         */
        private void doList(HttpServletRequest request, HttpServletResponse response) {
            // 准备一个容器，专门存储部门
            List<Dept> depts = new ArrayList<>();
        }
    }
    ```

  - 怎样将数据库中查询到的零散数据带到 JSP 中？

    - 将零散的数据封装成 Java 对象

      ```java
      Dept dept = new Dept();
      dept.setDeptno(deptno);
      dept.setDname(dname);
      dept.setLoc(loc);
      
      // 将部门对象放到 list 集合中
      depts.add(dept);
      ```

    - 将集合放到请求域中

      ```java
      request.setAttribute("deptList", depts);
      ```

    - 转发（不要重定向）

      ```java
      request.getRequestDispatcher("/list.jsp").forward(request, response);
      ```

- JSP 中

  - 从 request 域中取出 List 集合

  - 遍历 List 集合，取出每个部门对象，动态生成 tr

    ```jsp
    <%
        // 从 request 域中取出集合
        List<Dept> deptList = (List<Dept>) request.getAttribute("deptList");
        // 循环遍历
        int i = 0;
        for (Dept dept : deptList) {
    %>
    <tr>
        <td><%=++i%></td>
        <td><%=dept.getDeptno()%></td>
        <td><%=dept.getDname()%></td>
        <td>
            <a href='javascript:void(0)' onclick="del()">删除</a>
            <a href='edit.jsp'>修改</a>
            <a href='detail.jsp'>详情</a>
        </td>
    </tr>
    <%
        }
    %>
    ```

- 合并修改与详情超链接，减少代码复用

  - ```jsp
    <a href='<%=request.getContextPath()%>/dept/detail?f=edit&deptno=<%=dept.getDeptno()%>'>修改</a>
    <a href='<%=request.getContextPath()%>/dept/detail?f=detail&deptno=<%=dept.getDeptno()%>'>详情</a>
    ```

  - 修改 doDetail 方法中转发的代码，实现修改与详细页面共用同一段代码

    ```java
    String forward = "/" + request.getParameter("f") + ".jsp";
    request.getRequestDispatcher(forward).forward(request, response);
    ```

    





# JSP 的扩展名一定是 jsp 吗

- 不一定是 jsp，可以进行配置

- 在 CATALINA_HOME/conf/web.xml 中可以配置 jsp 文件的扩展名

  ```xml
  <servlet-mapping>
      <servlet-name>jsp</servlet-name>
      <url-pattern>*.jsp</url-pattern>
      <url-pattern>*.jspx</url-pattern>
  </servlet-mapping>
  ```

- xxx.jsp 文件对于服务器来说，只是一个普通的文本文件，web 容器会将 xxx.jsp 文件最终生成 Java 程序。最终执行时和 jsp 文件无关了







# 关于 JavaBean

- 我们可以创建 bean 包存储 JavaBean
- 什么是 JavaBean？
  - 可以理解为符合某种规范的 Java 类，比如：
    - 无参数构造方法
    - 属性私有化
    - 对外公开提供 get 和 set 方法
    - 实现 java.io.Serializable 接口
    - 重写 toString
    - 重写 hashCode + equals
    - ...
  - JavaBean 其实就是 Java 中的实体类，负责数据的封装







# 实现用户登录功能

> 当前 oa 项目存在的问题：任何人都可以进行 CRUD 这些危险的操作。所以需要加一个登录功能

- 实现登录功能

  - 步骤1：数据库中创建一个用户表：t_user

    - 存储用户的登录信息：用户名和密码

    - 密码一般在数据库中存储的是密文（这里先使用明文方式）

    - 用户表中插入数据

      ```sql
      drop table if exists t_user;
      
      CREATE TABLE t_user (
      	id INT PRIMARY KEY auto_increment,
          username varchar(255),
          password varchar(255)
      );
      
      INSERT INTO t_user(username, password) VALUES ('admin', '123456');
      INSERT INTO t_user(username, password) VALUES ('tom', '123321');
      ```

  - 步骤2：实现一个登录页面

    - 提供一个登陆的表单。有用户名和密码输入的框

    - 用户点击登录按钮，post 方式提交表单

      ```jsp
      <%@ page contentType="text/html;charset=UTF-8" %>
      <!DOCTYPE html>
      <html lang="en">
      <head>
          <meta charset="UTF-8">
          <title>欢迎使用OA系统</title>
      </head>
      <body>
      <h1>用户登录</h1>
      <hr>
      <form action="<%=request.getContextPath()%>/user/login" method="post">
          用户名：<input type="text" name="username"/><br>
          密码：<input type="password" name="password"/><br>
          <input type="submit" value="登录">
      </form>
      </body>
      </html>
      ```

  - 步骤3：后台提供一个 Servlet 类处理登录的请求

    - 登陆成功：跳转到部门列表页面

    - 登录失败：跳转到失败页面

      ```java
      @WebServlet("/user/login")
      public class UserServlet extends HttpServlet {
          @Override
          protected void doPost(HttpServletRequest request, HttpServletResponse response)
                  throws ServletException, IOException {
              // 获取用户名和密码
              String username = request.getParameter("username");
              String password = request.getParameter("password");
      
              // 连接数据库，验证用户名和密码
              Boolean flag = false;
              Connection conn = null;
              PreparedStatement ps = null;
              ResultSet rs = null;
              try {
                  conn = DBUtil.getConnection();
                  String sql = "select * from t_user where username=? and password=?";
                  ps = conn.prepareStatement(sql);
                  ps.setString(1, username);
                  ps.setString(2, password);
                  rs = ps.executeQuery();
                  if (rs.next()) {
                      flag = true;
                  }
              } catch (SQLException e) {
                  e.printStackTrace();
              } finally {
                  DBUtil.close(conn, ps, rs);
              }
      
              // 登录成功或失败
              if (flag) {
                  response.sendRedirect(request.getContextPath() + "/dept/list");
              } else {
                  response.sendRedirect(request.getContextPath() + "/error.jsp");
              }
          }
      }
      
      ```

  - 步骤4：再提供一个登陆失败的页面

- 存在的问题：

  - 目前的登录功能只是一个摆设，如果知道后端的请求路径，照样可以在不登陆的情况下访问
  - 这个登录没有真正起到拦截的作用
  
  需要 session 机制和 cookie 机制解决







# JSP 指令

- 指令的作用：指导 JSP 的翻译引擎如何工作

- 指令包括哪些？

  - include：包含指令，在 JSP 中完成静态包含，很少用了
  - taglib：引入标签库的指令。在 JSTL 部分学习
  - page：目前重点学习的指令

- 指令的使用语法

  - <%@指令名 属性名=属性值 属性名=属性值...%>

- page 指令当中的常用属性

  - ```jsp
    <%@page session="true|false" %>
    表示是否启动 session 对象
    默认值为 true
    ```

  - ```jsp
    <%@page contentType="text/html" %>
    设置响应的内容类型
    也可以同时设置字符集
    <%@page contentType="text/html;charset=UTF-8" %>
    ```

  - ```jsp
    <%@page pageEncoding="UTF-8" %>
    设置响应时采用的字符集
    ```

  - ```jsp
    <%@page import="java.util.*" %>
    import 语句，导包
    ```

  - ```jsp
    <%@page errorPage="/error.jsp" %>
    指定出错后的跳转路径
    ```

  - ```jsp
    <%@page isErrorPage="true" %>
    启用JSP内置对象：exception
    默认值为 false
    ```







# JSP 九大内置对象

- pageContext（jakarta.servlet.jsp.PageContext）   页面作用域
- request（jakarta.servlet.http.HttpServletRequest）   请求作用域
- session（jakarta.servlet.http.HttpSession）   会话作用域
- application（jakarta.servlet.ServletContext）   应用作用域
  - pageContext < request < session < application
  - 以上四个作用域都有：setAttribute、getAttribute、removeAttribute 方法
  - 使用原则：尽可能使用小的域



- exception（java.lang.Throwable）



- config（jakarta.servlet.ServletConfig）



- page（java.lang.Object）   其实是 this，当前的 servlet 对象



- out（jakarta.servlet.jsp.JspWriter）   负责输出
- response（jakarta.servlet.http.HttpServletResponse）   负责响应







# EL 表达式

- EL 表达式是什么
  - Expression Language（表达式语言）
  - EL 表达式可以代替 JSP 中的 java 代码，让 JSP 文件中的程序看起来更加整洁、美观
  - EL 表达式可以算是 JSP 语法的一部分，EL 表达式归属于 JSP

- EL 表达式出现在 JSP 中主要是：
  - 从某个作用域中取数据
  - 将取出的数据转换成字符串
  - 将字符串输出到浏览器上
  
- 语法格式

  - ${表达式}

- 例如：

  - 没有使用 EL 表达式

    ```jsp
    <%
    	request.setAttribute("username", "admin");
    %>
    
    <%= request.getAttribute("username") %>
    ```

  - 使用 EL 表达式

    ```jsp
    <%
    	request.setAttribute("username", "admin");
    %>
    
    ${username}
    ```

- EL 表达式的使用：

  ```jsp
  <%
  	// 创建 User 对象
  	User user = new User();
  	user.setUsername("tom");
  	user.setPassword("1234");
  
  	// 将 User 对象存储到 request 域中
  	request.setAttribute("userObj", user);
  %>
  
  <%--
      使用 EL 表达式获取 User 对象
      这里写的必须是存储到域对象中的 name
      底层：从域中取数据，取出 User 对象，然后调用 toSting 方法，转换成字符串，输出到浏览器
  --%>
  ${userObj}
  
  <%--输出对象属性值，前提是属性有对应的 get 方法--%>
  ${userObj.username}
  ${userObj.password}
  
  ${userObj["username"]}
  ${userObj["password"]}
  ```

  - ${xxx} 与 ${"xxx"} 的区别
    - ${xxx} 表示从某个域中取出数据，数据的 name 是 xxx，之前一定有这样的代码：域.setAttribute("xxx", 对象)
    - ${"xxx"} 表示直接将 xxx 作为普通字符串输出到浏览器
  - EL 表达式优先从小范围中取数据
    - pageContext < request < session < application
  - EL 表达式中有四个隐含的隐式的请求范围：
    - pageScope
    - requestScope
    - sessionScope
    - applicationScope
  - EL 表达式对 null 进行了预处理，如果是 null，则向浏览器输出一个空字符串
  - EL 表达式从 Map 集合中取数据
    - ${map.key}
  - EL 表达式从数组中取数据
    - ${array[index]}
  - EL 表达式获取应用的根路径
    - ${pageContext.request.contextPath}

- page 指令中，有一个属性，可以忽略 EL 表达式

  - ```jsp
    <%@page isElIgnored="true|false" %>
    ```

  - 以上是忽略整个页面的 EL 表达式

  - 如果想要忽略某个 EL 表达式，可以在表达式前加一个反斜杠 \

- EL 表达式的内置对象

  - pageContext
    - 与 JSP 中的 pageContext 是同一个对象
  - param
  - paramValues
  - initParam
  - 其他（非重点）

- EL 表达式中的运算符

  ```
  1.算术运算符
  	+ - * / %
  	在 EL 表达式中，+ 运算符只能做求和运算，不会进行字符串拼接
  2.关系运算符
  	== != > >= < <= eq
  	EL 表达式中，== 和 != 调用了 equals 方法
  3.逻辑运算符
  	! && || not and or
  4.条件运算符
  	? :
  5.取值运算符
  	[] .
  6.empty 运算符
  	判断是否为空，为空则 true
  ```







# JSTL 标签库

- 什么是 JSTL 表达式？

  - Java Standard Tag Lib（Java 标准的标签库）
  - JSTL 标签库通常结合 EL 表达式一起使用，目的是让 Java 代码消失
  - 标签是写在 JSP 当中的，但实际上还是要执行对应的 Java 程序（Java 程序在 jar 包中）

- 使用 JTSL 标签库步骤：

  - 第一步：引入 JSTL 标签库对应的 jar 包

    - tomcat 10之后引入的 jar 包是：
      - jakarta.servlet.jsp.jstl-2.0.0.jar
      - jakarta.servlet.jsp.jstl-api-2.0.0.jar
    - 在 IDEA 中怎么引入？
      - 在 WEB-INF 下新建 lib 目录，然后将 jar 包拷贝到 lib 当中，然后 “Add Lib...”
      - 与 mysql 的数据库驱动一样，都是放到 WEB-INF/lib 目录下
    - 什么时候要把 jar 包放到 WEB-INF/lib 目录下？该 jar 是 tomcat 服务器没有的

  - 第二步：在 JSP 中引入要使用的标签库

    - 使用 taglib 指令引入标签库

      <%@taglib prefix="" uri="" %>

      ```
      <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
      这个是核心标签库
      prefix="这里随便起名字，核心标签库，大家默认叫 c"
      ```

  - 第三步：在需要使用标签的位置使用即可

- JSTL 标签的原理

  - ```
    <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    以上 uri 后边的路径实际指向了 xxx.tld 文件
    tld 文件是一个 xml 配置文件
    在 tld 文件中描述了“标签”和“java 类”之间的关系
    c.tld 文件在 jakarta.servlet.jsp.jstl-2.0.0.jar 里边的 META-INF 目录下
    ```

  - 源码解析：配置文件 tld

    ```
    <tag>
        <description>
            对该标签的描述
        </description>
        <name>catch</name> 标签的名字
        <tag-class>org.apache.taglibs.standard.tag.common.core.CatchTag</tag-class> 标签对应的 java 类
        <body-content>JSP</body-content> 标签体当中可以出现的内容
        <attribute>
            <description>
    			对属性的描述
            </description>
            <name>var</name> 属性名
            <required>false</required> 属性是否为必须的
            <rtexprvalue>false</rtexprvalue> 属性值是否支持 EL 表达式
        </attribute>
    </tag>
    ```

- 核心标签库 core 中的常用标签

  - c:if

    - ```jsp
      <c:if test="boolean 类型，支持 EL 表达式"></c:if>
      ```

  - c:forEach

    - ```jsp
      <c:forEach items="集合，支持 EL 表达式" var="集合中的元素" varStatus="元素状态对象">
      	${元素状态对象.count}
      </c:forEach>
      ```

    - ```
      <c:forEach var="i" begin="1" end="10" step="2">
      	${i}
      </c:forEach>
      ```

  - c:choose c:when c:otherwise

    - ```jsp
      <c:choose>
      	<c:when test="${param.age < 18}">
          	青少年
          </c:when>
      	<c:when test="${param.age < 35}">
          	青年
          </c:when>
          <c:when test="${param.age < 55}">
          	中年
          </c:when>
      	<c:otherwise>
          	老年
          </c:otherwise>
      </c:choose>
      ```







# EL + JSTL 改造 oa 项目

- 引入 jar 包

- EL 表达式 + JSTL 改造代码：部分代码举例

  - 部门详情

    ```jsp
    <%-- old --%>
    
    <%@ page import="com.shameyang.oa.bean.Dept" %>
    <%@ page contentType="text/html;charset=UTF-8" %>
    
    <%
        // 从 request 域中取出数据
        Dept deptInfo = (Dept)request.getAttribute("deptInfo");
    %>
    
    部门编号：<%=deptInfo.getDeptno()%><br>
    部门名称：<%=deptInfo.getDname()%><br>
    部门位置：<%=deptInfo.getLoc()%><br>
    ```

    ```jsp
    <%-- new --%>
    
    <%@ page contentType="text/html;charset=UTF-8" %>
    
    部门编号：${deptInfo.deptno}<br>
    部门名称：${deptInfo.dname}<br>
    部门位置：${deptInfo.loc}<br>
    ```

  - 部门列表

    ```jsp
    <%-- old --%>
        
    <%@ page import="java.util.List" %>
    <%@ page import="com.shameyang.oa.bean.Dept" %>
    <%@page contentType="text/html;charset=UTF-8" %>
    
    <%
    	// 从 request 域中取出集合
    	List<Dept> deptList = (List<Dept>) request.getAttribute("deptList");
    	// 循环遍历
    	int i = 0;
    	for (Dept dept : deptList) {
    %>
        
    	<%=++i%>
    	<%=dept.getDeptno()%>
    	<%=dept.getDname()%>
    	<a href='javascript:void(0)' onclick="del(<%=dept.getDeptno()%>)">删除</a>
    	<a href='<%=request.getContextPath()%>/dept/detail?f=edit&deptno=<%=dept.getDeptno()%>'>修改</a>
    	<a href='<%=request.getContextPath()%>/dept/detail?f=detail&deptno=<%=dept.getDeptno()%>'>详情</a>
        
    <%
    	}
    %>
    ```

    ```jsp
    <%-- new --%>
    
    <%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
    <%@page contentType="text/html;charset=UTF-8" %>
    
    <c:forEach items="${deptList}" varStatus="deptStatus" var="dept">
    	${deptStatus.count}
    	${dept.deptno}
    	${dept.dname}
    	<a href='javascript:void(0)' onclick="del(${dept.deptno})">删除</a>
    	<a href='<%=request.getContextPath()%>/dept/detail?f=edit&deptno=${dept.deptno}'>修改</a>
    	<a href='<%=request.getContextPath()%>/dept/detail?f=detail&deptno=${dept.deptno}'>详情</a>
    </c:forEach>
    ```

- base 标签改造

  - HTML 中的 `<base>` 标签可以设置网页的基础路径，这样我们在超链接中的路径就可以变得简洁了
  - base 标签通常出现在 head 标签中
  - `<base href="http://localhost:8080/oa/">`
  - 在页面中，没有以 / 开始的路径，都会自动在路径前加上 base 中的路径
  - 注意：JS 代码中最好不要依赖 base 标签，使用全路径
  
  ```jsp
  <base href="${pageContext.request.scheme}://${pageContext.request.serverName}:${pageContext.request.serverPort}${pageContext.request.contextPath}/">
  ```
  
  