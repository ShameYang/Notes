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
  <% @page contentType="text/html;charset=UTF-8" %>
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
- <% @page contentType="text/html;charset=UTF-8" %>
  - page 指令，通过 contentType 属性设置响应的类型







# Servlet + JSP 改造 oa 项目

> Servlet 收集数据（处理业务），JSP 展示数据

- 将之前准备的 html 文件改为 jsp 文件，完成所有页面的正常流转（修改超链接的请求路径）

  - 例如 list.jsp

    ```jsp
    <% @ page contentType="text/html;charset=UTF-8" %>
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
    <% @ page contentType="text/html;charset=UTF-8" %>
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







# JavaBean

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