# JDBC 基本概念

- 什么是 JDBC

  Java DataBase Connectivity，在 Java 语言中编写 SQL 语句，对数据库中数据进行 CRUD 操作

  

- JDBC 相关类库在哪里

  java.sql.*

  

- JDBC 本质

  本质上是 java.sql 包下的一堆接口，由 SUN 公司制定

  JDBC 是体现接口作用的典型案例，降低了耦合度，提高了扩展力

  无需关注哪个数据库，直接面向 JDBC 接口编程







# JDBC 编程步骤

前置工作：在项目下创建一个 lib 文件夹，将 mysql.jar 拷贝到该目录下，右键 add as library（等同于为 jar 包配置环境变量）



1. 注册驱动 - 加载 Driver 类

   通知 java 程序要连接哪个数据库

   ```java
   //第一种方式
   DriverManager.deregisterDriver(new com.mysql.cj.jdbc.Driver()); // MySQL 8.0 写法
   
   //第二种方式：类加载注册
   Class.forName(com.mysql.cj.jdbc.Driver); // MySQL 8.0 写法
   ```

   

2. 获取数据库连接 - 得到 Connection

   开启 java 进程和 mysql 进程间的通道

   ```java
   conn = DriverManager.getConnection(String url, String user, String password);
   ```

   

3. 获取数据库操作对象

   这个对象用来执行 SQL语句

   ```java
   stmt = conn.createStatement();
   ```

   

4. 执行 SQL 语句

   执行 CRUD 操作

   ```java
   //DML
   String sql = "DML 语句";
   stmt.execute(sql);
   
   //DQL
   String sql = "select ...";
   rs = stmt.executeQuery(sql);
   ```

   

5. 处理查询结果集

   如果第 4 步是 select 语句，才有这一步

   ```java
   while (rs.next()) {
   	String name = rs.getString("字段名");
   	System.out.println(name);
   }
   ```

   

6. 释放资源

   ```java
   //rs 可替换为任何要释放的对象
   if (rs != null) {
   	try {
   		rs.close();
   	} catch (SQLException e) {
   		e.printStackTrace();
   	}
   }
   ```







# JDBC 编程完整代码

## DML

```java
package com.shameyang.jdbc;

import java.sql.*;

/**
 * @author ShameYang
 * @date 2023/6/11 20:49
 * @description 使用 IDEA 开发一个 JDBC 程序
 */
public class JDBC01 {
    public static void main(String[] args) throws SQLException {
        //分别创建 Connection、Statement、ResultSet 对象
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //1.注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver"); //这里是 MySQL 8.0 写法
            //2.获取连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc", "root", "123456");
            //3.获取数据库操作对象
            stmt = conn.createStatement();
            //4.执行 SQL
            String sql = "insert into student(id, name, sex) values(01, '张三', '男')";
			stmt.execute(sql);
            }
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            //5.释放资源
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```



## DQL

```java
package com.shameyang.jdbc;

import java.sql.*;

/**
 * @author ShameYang
 * @date 2023/6/11 20:49
 * @description 使用 IDEA 开发一个 JDBC 程序
 */
public class JDBC01 {
    public static void main(String[] args) throws SQLException {
        //分别创建 Connection、Statement、ResultSet 对象
        Connection conn = null;
        Statement stmt = null;
        ResultSet rs = null;
        try {
            //1.注册驱动
            Class.forName("com.mysql.cj.jdbc.Driver"); //这里是 MySQL 8.0 写法
            //2.获取连接
            conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/jdbc", "root", "123456");
            //3.获取数据库操作对象
            stmt = conn.createStatement();
            //4.执行 SQL
            String sql = "select name as '学生姓名' from student where id = 01";
            rs = stmt.executeQuery(sql);
            //5.处理查询结果集
            while (rs.next()) {
                String name = rs.getString("学生姓名");
                System.out.println(name);
            }
        } catch (SQLException | ClassNotFoundException e) {
            e.printStackTrace();
        } finally {
            //6.释放资源
            if (rs != null) {
                try {
                    rs.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (stmt != null) {
                try {
                    stmt.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
            if (conn != null) {
                try {
                    conn.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }
        }
    }
}

```

