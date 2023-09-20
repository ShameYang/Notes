# 一、MyBatis 概述

## 1.1  框架

- 框架，framework，本质上是对通用代码进行封装，提前写好一套接口和类，在写项目时直接引入这些接口和类（引入框架），提高开发效率
- Java 常用框架
  - SSM：Spring + SpringMVC + MyBatis
  - SpringBoot
  - SpringCloud
  - ......
- 框架一般以 jar 包形式存在（jar 包中有 class 文件和各种配置文件）



## 1.2 三层架构

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E4%B8%89%E5%B1%82%E6%9E%B6%E6%9E%84.png" style="zoom:67%;float:left" />

- 表现层（UI）：①接收前端请求，②返回 json 数据给前端
- 业务逻辑层（BLL）：①处理表现层转发的前端请求（具体业务），②将持久层获取的数据返回到表现层
- 数据访问层（DAL）：操作数据库完成 CRUD，并将数据返回给上一层（业务逻辑层）
- MyBatis 就是持久层框架



## 1.3 JDBC 的不足

- SQL 语句写死在 Java 程序中，不灵活。修改 SQL 的话就要修改 Java 代码，违背 OCP 原则
- 给 ? 传值很繁琐
- 将结果集封装成 Java 对象也很繁琐



## 1.4 了解 MyBatis

- MyBatis 本质上是对 JDBC 的封装，通过 MyBatis 完成 CRUD
- MyBatis 属于持久层框架
- ORM
  - Object：JVM 中的 Java 对象
  - Relational：关系型数据库
  - Mapping：将 JVM 中的 Java 对象映射到数据库表中一行记录，或将数据库表中一行记录映射成 JVM 中的一个 Java 对象
  - <img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/ORM%E6%80%9D%E6%83%B3.png" style="zoom:67%;float:left" />
- MyBatis 就是一个 ORM 框架，是半自动的 ORM，因为 SQL 语句需要程序员自己编写
- MyBatis 框架特点
  - 支持定制化 SQL、存储过程、基本映射以及高级映射
  - 避免了几乎所有的 JDBC 代码手动设置参数以及获取结果集
  - 支持 XML 开发，也支持注解式开发（XML 方式使用较多，保证了 sql 语句的灵活）
  - 通过接口，将对象和数据相互转化
  - 体积小：两个 jar 包，两个 XML 配置文件
  - sql 解耦合
  - 提供了基本映射和高级映射标签
  - 提供了 XML 标签，支持动态 SQL 的编写
  - ......







# 二、MyBatis 入门程序

## 2.1 环境

- 软件
  - intelliJ IDEA：2023.2.1
  - Navicat for MySQL：16.0.14
  - MySQL 数据库：8.0.33

- 组件
  - MySQL 驱动：8.0.33
  - MyBatis：3.5.13
  - JDK：Java 17
  - JUnit
  - Logback



## 2.2 入门程序开发步骤

- 准备数据库表

- IDEA --> new project，配置 JDK

- 新建一个 Maven 模块

  - 引入 mybatis 依赖和 mysql 依赖

    ```xml
    <dependencies>
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.13</version>
        </dependency>
    
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <version>8.0.33</version>
        </dependency>
    </dependencies>
    ```

  - 在 src/main/resources 中编写 mybatis 核心配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
      PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
      <environments default="development">
        <environment id="development">
          <transactionManager type="JDBC"/>
          <dataSource type="POOLED">
            <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
            <property name="url" value="jdbc:mysql://localhost:3306/formybatis"/>
            <property name="username" value="root"/>
            <property name="password" value="123456"/>
          </dataSource>
        </environment>
      </environments>
      <mappers>
        <!-- 指定 XxxMapper.xml 的路径 -->
        <mapper resource="XxxMapper.xml"/>
      </mappers>
    </configuration>
    ```

  - 在 src/main/resources 中编写 SQL 语句的配置文件，XxxMapper.xml（一张表对应一个）

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
      PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="org.mybatis.example.BlogMapper">
    
    </mapper>
    ```

- 编写 MyBatis 程序

  - SqlSessionFactoryBuilder --> SqlSessionFactory --> SqlSession

    ```java
    // 获取 SqlSessionFactoryBuilder 对象
    SqlSessionFactoryBuilder ssfb = new SqlSessionFactoryBuilder();
    
    // 获取 SqlSessionFactory 对象
    InputStream is = Resources.getResourceAsStream("mybatis 核心配置文件的路径"); // 默认从类的根路径下开始查找
    SqlSessionFactory ssf = ssfb.build(is); // 一个数据库对应一个 SqlSessionFactory 对象
    
    // 获取 SqlSession 对象
    SqlSession sqlSession = ssf.openSession();
    
    // 执行 SQL 语句
    sqlSession.insert("SQL 语句的配置文件中对应的 id"); // 返回值：影响数据库表中的记录条数
    
    // 手动提交
    sqlSession.commit();
    ```

    
