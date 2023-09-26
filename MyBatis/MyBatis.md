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

- 表现层（UI）：① 接收前端请求，② 返回 json 数据给前端
- 业务逻辑层（BLL）：① 处理表现层转发的前端请求（具体业务），② 将持久层获取的数据返回到表现层
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

  - ① 引入 mybatis 依赖和 mysql 依赖

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

  - ② 在 src/main/resources 中编写 mybatis 核心配置文件

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
      PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
      <environments default="development">
        <environment id="development">
          <transactionManager type="JDBC"/> // 事务管理
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

  - ③ 在 src/main/resources 中编写 SQL 语句的配置文件，XxxMapper.xml（一张表对应一个），以 insert 语句为例

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
      PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
      "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="org.mybatis.example.BlogMapper">
        <insert id="xxx">
        	<!-- insert 语句 -->
        </insert>
    </mapper>
    ```

  - ④ 编写 MyBatis 程序

    - SqlSessionFactoryBuilder --> SqlSessionFactory --> SqlSession

      ```java
      // 获取 SqlSessionFactoryBuilder 对象
      SqlSessionFactoryBuilder ssfb = new SqlSessionFactoryBuilder();
      
      // 获取 SqlSessionFactory 对象
      // 一个数据库对应一个 SqlSessionFactory 对象，通常一个该对象对应一个数据库
      InputStream is = Resources.getResourceAsStream("mybatis 核心配置文件的路径"); // 默认从类的根路径下开始查找
      SqlSessionFactory ssf = ssfb.build(is);
      
      // 获取 SqlSession 对象
      SqlSession sqlSession = ssf.openSession();
      
      // 执行 SQL 语句
      sqlSession.insert("SQL 语句的配置文件中对应的 id"); // 返回值：影响数据库表中的记录条数
      
      // 手动提交
      sqlSession.commit();
      ```



## 2.3 完整的 Mybatis 程序

```java
public class FirstMybatis {
    public static void main(String[] args) {
        SqlSession sqlSession = null;
        try {
            SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(Resources.getResourceAsStream("mybatis-config.xml"));
            // 开启会话（底层开启事务机制）
            sqlSession = sqlSessionFactory.openSession();
            // 执行 SQL 语句，处理相关业务
            int count = sqlSession.insert("insertStu");
            System.out.println(count);
            // 没有发生异常时提交事务
            sqlSession.commit();
        } catch (IOException e) {
            if (sqlSession != null) {
                sqlSession.rollback();
            }
        } finally {
            if (sqlSession != null) {
                sqlSession.close();
            }
        }
    }
}
```





## 2.4 引入日志框架 logback

> 引入日志框架可以看清楚 mybatis 执行的具体 sql

- 在配置文件 \<configuration> 中添加 \<settings>，参考 Maven 手册

  - STDOUT_LOGGING 是标准日志，mybatis 已经实现了这种标准日志
  - 在使用其他日志时，需要引入相应的依赖

  ```xml
  <configuration>
      <settings>
  		<setting name="logImpl" value="STDOUT_LOGGING"></setting>
  	</settings>
  </configuration>
  ```

- ① 核心配置文件中添加 \<settings>

  ```xml
  <settings>
      <setting name="logImpl" value="SLF4J"/>
  </settings>
  ```

- ② 引入 logback 依赖，这个日志框架实现了 slf4j 规范

  ```xml
  <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.11</version>
      <scope>test</scope> <!-- 注意这里的范围 -->
  </dependency>
  ```

- ③ 引入 logback 配置文件

  - 该文件必须放在类的根路径下，即 src/main/resources 目录下
  - 配置文件名必须为 logback-test 或 logback

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  
  <!-- 配置文件修改时重新加载，默认true -->
  <configuration scan="false">
      <!-- 控制台输出 -->
      <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
          <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
              <!-- 日志输出格式 -->
              <pattern>%d{yyyy-MM-dd HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n</pattern>
          </encoder>
      </appender>
  
      <!-- mybatis log configure -->
      <logger name="com.apache.ibatis" level="TRACE"/>
      <logger name="java.sql.Connection" level="DEBUG"/>
      <logger name="java.sql.Statement" level="DEBUG"/>
      <logger name="java.sql.PreparedStatement" level="DEBUG"/>
  
      <!-- 日志输出级别，TRACE < DEBUG < INFO < WARN < ERROR -->
      <root level="DEBUG">
          <appender-ref ref="STDOUT"/>
          <appender-ref ref="FILE"/>
      </root>
  </configuration>
  ```



## 2.5 封装 mybatis 工具类

```java
public class SqlSessionUtil {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private SqlSessionUtil() {
    }

    /**
     * 获取会话对象
     * @return 会话对象
     */
    public static SqlSession openSession() {
        return sqlSessionFactory.openSession();
    }
}
```







# 三、使用 MyBatis 完成 CRUD

> JDBC 代码中占位符采用的是 ?
>
> 在 MyBatis 中是 #{ }



## 3.1 insert（Create）

- Mapper 中使用 #{ }  占位，不将 sql 写死

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="org.mybatis.example.BlogMapper">
      <insert id="insertStu">
          insert into t_student(sno, sname, ssex)
          VALUES (null, #{sname}, #{ssex});
      </insert>
  </mapper>
  ```

- 使用 Map 集合，动态传参。Mapper 的 #{ } 中需要写 map 集合中的 key（底层调用了 map.get(key)）

  ```java
  public class StuMapperTest {
      @Test
      public void testInsert() {
          Map<String, Object> map = new HashMap<>();
          map.put("sname", "mike");
          map.put("ssex", "man");
  
          SqlSession sqlSession = SqlSessionUtil.openSession();
          sqlSession.insert("insertStu", map);
          sqlSession.commit();
          sqlSession.close();
      }
  }
  ```

- 也可以使用 bean 类完成传参，将 Mapper 的 #{ } 中修改为属性名即可（底层调用了 bean 类的 get 方法，严格来说：#{ } 中传的参数是 get 方法名去掉 get，剩下的全小写）



## 3.2 delete（Delete）

- StuMapper.xml

  ```xml
  <delete id="deleteStu">
  	delete from t_student where sno = #{sno};
  </delete>
  ```

- 测试删除方法

  ```java
  @Test
  public void testDelete() {
      SqlSession sqlSession = SqlSessionUtil.openSession();
      sqlSession.delete("deleteStu", 11);
      sqlSession.commit();
      sqlSession.close();
  }
  ```



## 3.3 update（Update）

- StuMapper.xml

  ```xml
  <update id="updateStu">
      update t_student set sno = #{sno}, sname = #{sname}, ssex = #{ssex} where sno = #{sno};
  </update>
  ```

- 测试修改方法

  ```java
  @Test
  public void testUpdate() {
      SqlSession sqlSession = SqlSessionUtil.openSession();
      Student student = new Student(10L, "Alice","woman" );
      sqlSession.update("updateStu", student);
      sqlSession.commit();
      sqlSession.close();
  }
  ```



## 3.4 select（Retrieve）

- StuMapper.xml

  - 注意：必须写 resultType 属性，不然 mybatis 无法确定返回值类型，就会报错

  ```xml
  <!-- 查询一条 -->
  <select id="selectStu" resultType="com.shameyang.mybatis.bean.Student">
      select * from t_student where sno = #{sno};
  </select>
  
  <!-- 查询多条，不需要加 where -->
  <select id="selectAll" resultType="com.shameyang.mybatis.bean.Student">
      select * from t_student;
  </select>
  ```

- 测试查询一条记录

  ```java
  @Test
  public void testSelect() {
      SqlSession sqlSession = SqlSessionUtil.openSession();
      sqlSession.selectOne("selectStu", 1);
      sqlSession.close();
  }
  ```

- 测试查询多条记录

  ```java
  @Test
  public void testSelectAll() {
      SqlSession sqlSession = SqlSessionUtil.openSession();
      List<Student> students =  sqlSession.selectList("selectAll");
      students.forEach(System.out::println); // lambda 表达式
      sqlSession.close();
  }
  ```



## 3.5 SQL Mapper 的 namespace

> SQL Mapper 配置文件中，\<mapper> 标签的 namespace 属性翻译为命名空间，主要是为了防止 sqlId 冲突

- xml 文件中

  ```xml
  <mapper namespace="命名空间">
      <insert id="insertStu">
          insert into t_student(sno, sname, ssex)
          VALUES (null, #{sname}, #{ssex});
      </insert>
      ...
  </mapper>
  ```

- Java 程序中

  ```java
  sqlSession.insert("命名空间.insertStu");
  ```







# 四、MyBatis 核心配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!-- resource 从类的根路径下查找，即 src/main/resources -->
    <properties resource="jdbc.properties"/>
    
    <!-- 环境配置，default 表示默认的环境 -->
    <environments default="development">
        <!-- 
			其中的一个环境，连接的数据库是 formybatis
        	一般一个数据库对应一个 environment环境，一个环境对应一个 SqlSessionFactory 对象
		-->
        <environment id="development">
            <!-- 
				指定事务管理的方式
 					JDBC：使用原生代码管理事务
					MANAGED：mybatis 不负责管理事务，交给其它的 JEE（JavaEE） 容器来负责，例如 spring
			-->
            <transactionManager type="JDBC|MANAGED"/>
            <!-- 
				1.数据源，为程序提供 Connection 对象
				2，实际上是一套规范，这套接口由 JDK 规定
				3.实现 javax.sql.DataSource 接口即可就能编写自己的数据源组件，例如数据库连接池
				4.常见的数据源组件：
					阿里的德鲁伊连接池 druid
					c3p0
					dbcp ...
				5.type 属性
					UNPOOLED：不使用数据库连接池技术。每个请求都创建一个 Connection 对象
					POOLED：使用 mybatis 自己实现的数据库连接池
					JNDI：集成第三方的数据库连接池
			-->
            <dataSource type="UNPOOLED|POOLED|JNDI">
                <property name="driver" value="{jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.user}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    
    <mappers>
        <mapper resource="StudentMapper.xml"/>
    </mappers>
    
</configuration>
```







# 五、WEB 应用中使用 MyBatis（MVC 架构模式）

## 5.1 需求分析

银行转账业务，需要转出账户、转入账户、转账金额

## 5.2 数据库表的设计和数据准备

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/WEB-%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A1%A8%E7%9A%84%E8%AE%BE%E8%AE%A1.png" style="zoom: 67%;float:left" />



<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/WEB-%E6%95%B0%E6%8D%AE%E5%BA%93%E8%A1%A8%E6%95%B0%E6%8D%AE.png" style="zoom: 80%;float:left" />



## 5.3 实现步骤

### 第一步：环境搭建

- IDEA 中创建 Maven WEB 应用

- IDEA 配置 Tomcat，然后应用部署到 tomcat

- 手动添加 java 目录

- web.xml 文件的版本比较低，可以从 tomcat 10的样例文件中复制，然后修改

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <web-app xmlns="https://jakarta.ee/xml/ns/jakartaee"
           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="https://jakarta.ee/xml/ns/jakartaee
                        https://jakarta.ee/xml/ns/jakartaee/web-app_6_0.xsd"
           version="6.0"
           metadata-complete="false">
  </web-app>
  ```

- 删除 index.jsp 文件，我们这个项目只使用 html

- 配置 pom.xml 文件

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
           xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
      <modelVersion>4.0.0</modelVersion>
  
      <groupId>com.shameyanng</groupId>
      <artifactId>mybatis-003-web</artifactId>
      <packaging>war</packaging>
      <version>1.0-SNAPSHOT</version>
  
      <name>mybatis-003-web</name>
      <url>http://localhost:8080/bank</url>
  
      <dependencies>
          <!-- mybatis -->
          <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis</artifactId>
              <version>3.5.13</version>
          </dependency>
          <!-- mysql -->
          <dependency>
              <groupId>com.mysql</groupId>
              <artifactId>mysql-connector-j</artifactId>
              <version>8.0.33</version>
          </dependency>
          <!-- logback -->
          <dependency>
              <groupId>ch.qos.logback</groupId>
              <artifactId>logback-classic</artifactId>
              <version>1.4.11</version>
          </dependency>
          <!-- servlet -->
          <dependency>
              <groupId>jakarta.servlet</groupId>
              <artifactId>jakarta.servlet-api</artifactId>
              <version>6.0.0</version>
              <scope>provided</scope>
          </dependency>
      </dependencies>
  
      <build>
          <finalName>mybatis-003-web</finalName>
      </build>
  </project>
  ```

- 引入相关配置文件，放到 resources 目录下（类的根路径）

  - mybatis-config.xml

    ````xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE configuration
            PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
        <properties resource="jdbc.properties"/>
        
        <settings>
            <setting name="logImpl" value="SLF4J"/>
        </settings>
        
        <environments default="development">
            <environment id="development">
                <transactionManager type="JDBC"/>
                <dataSource type="POOLED">
                    <property name="driver" value="${jdbc.driver}"/>
                    <property name="url" value="${jdbc.url}"/>
                    <property name="username" value="${jdbc.username}"/>
                    <property name="password" value="${jdbc.password}"/>
                </dataSource>
            </environment>
        </environments>
        
        <mappers>
            <!-- 一定要注意这里的路径！！！ -->
            <mapper resource="AccountMapper.xml"/>
        </mappers>
    </configuration>
    ````

  - AccountMapper.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE mapper
            PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
            "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="account">
    
    </mapper>
    ```

  - logback.xml

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <configuration scan="false">
        <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
            <encoder class="ch.qos.logback.classic.encoder.PatternLayoutEncoder">
                <pattern>[%thread] %-5level %logger{36} - %msg%n</pattern>
            </encoder>
        </appender>
    
        <logger name="com.apache.ibatis" level="TRACE"/>
        <logger name="java.sql.Connection" level="DEBUG"/>
        <logger name="java.sql.Statement" level="DEBUG"/>
        <logger name="java.sql.PreparedStatement" level="DEBUG"/>
    
        <root level="DEBUG">
            <appender-ref ref="STDOUT"/>
            <appender-ref ref="FILE"/>
        </root>
    </configuration>
    ```

  - jdbc.properties

    ```properties
    jdbc.driver=com.mysql.cj.jdbc.Driver
    jdbc.url=jdbc:mysql://localhost:3306/formybatis
    jdbc.username=root
    jdbc.password=123456
    ```



### 第二步：前端页面 index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>银行账户转账</title>
</head>
<body>
<!-- /bank是应用的根，部署web应用到tomcat的时候一定要注意这个名字 -->
<form action="/bank/transfer" method="post">
    转出账户：<input type="text" name="fromActno"/><br>
    转入账户：<input type="text" name="toActno"/><br>
    转账金额：<input type="text" name="money"/><br>
    <input type="submit" value="转账"/>
</form>
</body>
</html>
```



### 第三步：MVC 架构模式分层

在 src/main/java 包下创建 bean 包、service 包、dao 包、web 包、utils 包

```
...
 |---java
 	  |---com.shameyang.bank
 			|---bean
 			|---dao
				 |---impl
 			|---service
 				 |---impl
 			|---utils
 			|---web
 			|---exception
```



### 第四步：拷贝之前封装的工具类

```java
public class SqlSessionUtil {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private SqlSessionUtil() {
    }

    /**
     * 获取会话对象
     * @return 会话对象
     */
    public static SqlSession openSession() {
        return sqlSessionFactory.openSession();
    }
}
```



### 第五步：定义 bean 类：Account

```java
package com.shameyang.bank.bean;


public class Account {
    private Long id;
    private String actno;
    private Double balance;

    public Account(Long id, String actno, Double balance) {
        this.id = id;
        this.actno = actno;
        this.balance = balance;
    }

    public Account() {
    }

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", actno='" + actno + '\'' +
                ", balance=" + balance +
                '}';
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getActno() {
        return actno;
    }

    public void setActno(String actno) {
        this.actno = actno;
    }

    public Double getBalance() {
        return balance;
    }

    public void setBalance(Double balance) {
        this.balance = balance;
    }
}
```



### 第六步：编写 AccountDao 接口，以及实现类

dao 中需要提供的方法：

- 转帐前查询余额是否充足：selectByActno
- 转账时需要更新账户：update



AccountDao

```java
package com.shameyang.bank.dao;

import com.shameyang.bank.bean.Account;

public interface AccountDao {
    /**
     * 根据账号获取账户信息
     * @param actno 账号
     * @return 账户信息
     */
    Account selectByActno(String actno);

    /**
     * 更新账户信息
     * @param act 账户信息
     * @return 1 表示成功，其他值则失败
     */
    int update(Account act);
}
```



AccountDaoImpl

```java
package com.shameyang.bank.dao.impl;

import com.shameyang.bank.bean.Account;
import com.shameyang.bank.dao.AccountDao;
import com.shameyang.bank.utils.SqlSessionUtil;
import org.apache.ibatis.session.SqlSession;

public class AccountDaoImpl implements AccountDao {
    @Override
    public Account selectByActno(String actno) {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        Account act = (Account) sqlSession.selectOne("selectByActno", actno);
        sqlSession.close();
        return act;
    }

    @Override
    public int update(Account act) {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        int count = sqlSession.update("update", act);
        sqlSession.commit();
        sqlSession.close();
        return count;
    }
}

```



### 第七步：将Dao实现类编写的 mybatis 代码映射到 AccountMapper

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="account">
    <select id="selectByActno" resultType="com.shameyang.bank.bean.Account">
        select * from t_act where actno= #{actno};
    </select>
    <update id="update">
        update t_act set balance = #{balance} where actno = #{actno};
    </update>
</mapper>
```



### 第八步：编写 AccountService 接口以及实现类

异常类

- 余额不足

  ```java
  package com.shameyang.bank.exception;
  
  public class MoneyNotEnoughException extends Exception {
      public MoneyNotEnoughException() {
      }
  
      public MoneyNotEnoughException(String message) {
          super(message);
      }
  }
  ```

- 应用异常

  ```java
  package com.shameyang.bank.exception;
  
  public class AppException extends Exception {
      public AppException() {
          super();
      }
  
      public AppException(String message) {
          super(message);
      }
  }
  ```

  

AccountService

```java
package com.shameyang.bank.service.impl;

import com.shameyang.bank.exception.AppException;
import com.shameyang.bank.exception.MoneyNotEnoughException;

public interface AccountService {
    /**
     * 银行账户转账
     * @param fromActno 转出账户
     * @param toActno 转入账户
     * @param money 转账金额
     * @throws MoneyNotEnoughException 余额不足异常
     * @throws AppException App 发生异常
     */
    void transger(String fromActno, String toActno, double money)
            throws MoneyNotEnoughException, AppException;
}
```



AccountServiceImpl

```java
package com.shameyang.bank.service.impl;

import com.shameyang.bank.bean.Account;
import com.shameyang.bank.dao.AccountDao;
import com.shameyang.bank.dao.impl.AccountDaoImpl;
import com.shameyang.bank.exception.AppException;
import com.shameyang.bank.exception.MoneyNotEnoughException;
import com.shameyang.bank.service.AccountService;

public class AccountServiceImpl implements AccountService {
    AccountDao accountDao = new AccountDaoImpl();

    @Override
    public void transfer(String fromActno, String toActno, double money)
            throws MoneyNotEnoughException, AppException {
        Account fromAct = accountDao.selectByActno(fromActno);
        if (fromAct.getBalance() < money) {
            throw new MoneyNotEnoughException("对不起，您的余额不足");
        }
        try {
            Account toAct = accountDao.selectByActno(toActno);
            fromAct.setBalance(fromAct.getBalance() - money);
            toAct.setBalance(fromAct.getBalance() + money);
            // 更新数据库
            accountDao.update(fromAct);
            accountDao.update(toAct);
        } catch (Exception e) {
            throw new AppException("转账失败，未知原因！请联系管理员！");
        }
    }
}
```



### 第九步：编写 AccountController

```java
package com.shameyang.bank.web.controller;

import com.shameyang.bank.exception.AppException;
import com.shameyang.bank.exception.MoneyNotEnoughException;
import com.shameyang.bank.service.AccountService;
import com.shameyang.bank.service.impl.AccountServiceImpl;
import jakarta.servlet.ServletException;
import jakarta.servlet.annotation.WebServlet;
import jakarta.servlet.http.HttpServlet;
import jakarta.servlet.http.HttpServletRequest;
import jakarta.servlet.http.HttpServletResponse;

import java.io.IOException;
import java.io.PrintWriter;

/**
 * @author ShameYang
 * @date 2023/9/24 11:42
 * @description 账户控制器
 */
@WebServlet("/transfer")
public class AccountController extends HttpServlet {
    AccountService accountService = new AccountServiceImpl();

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response)
            throws ServletException, IOException {
        // 获取响应流
        response.setContentType("text/html");
        PrintWriter out = response.getWriter();
        // 获取账户信息
        String fromActno = request.getParameter("fromActno");
        String toActno = request.getParameter("toActno");
        double money = Double.parseDouble(request.getParameter("money"));
        // 调用业务方法完成转账
        try {
            accountService.transfer(fromActno, toActno, money);
            out.print("<h1>转账成功！</h1>");
        } catch (MoneyNotEnoughException | AppException e) {
            out.print(e);
        }
    }
}
```



### 最后一步：测试

启动服务器，打开浏览器，输入 pom.xml 文件中设置的地址：http://localhost:8080/bank



404 问题：检查 web.xml 文件中 metadata-complete="true"，如果为 true，表示只支持配置文件，无法注解式开发，改成 false 即可，同时支持配置文件和注解





## 5.4 事务问题

在之前的转账业务中，更新了两个账户，我们需要保证同时成功或失败，这时需要事务机制，在 transfer 方法开始执行时开启事务，直到两个更新都成功，再提交事务



做出如下修改：

```java
public class AccountServiceImpl implements AccountService {
    AccountDao accountDao = new AccountDaoImpl();

    @Override
    public void transfer(String fromActno, String toActno, double money)
            throws MoneyNotEnoughException, AppException {
        Account fromAct = accountDao.selectByActno(fromActno);
        if (fromAct.getBalance() < money) {
            throw new MoneyNotEnoughException("对不起，您的余额不足");
        }
        try {
            Account toAct = accountDao.selectByActno(toActno);
            fromAct.setBalance(fromAct.getBalance() - money);
            toAct.setBalance(toAct.getBalance() + money);
            // 更新数据库（添加事务）
            SqlSession sqlSession = SqlSessionUtil.openSession();
            accountDao.update(fromAct);
            // 模拟异常
            String s = null;
            s.toString();
            accountDao.update(toAct);
            sqlSession.commit();
            sqlSession.close();
        } catch (Exception e) {
            throw new AppException("转账失败，未知原因！请联系管理员！");
        }
    }
}
```



我们再次执行程序后，会发现转账失败，但是转账金额消失了！

- 原因： service 和 dao 中使用的 SqlSession 对象不是同一个
- 解决方法：我们可以将 SqlSession 对象存放到 ThreadLocal 当中，保证 service 和 dao 的 SqlSession 对象是同一个



修改 SqlSessionUtil

```java
public class SqlSessionUtil {
    private static SqlSessionFactory sqlSessionFactory;

    static {
        try {
            sqlSessionFactory = new SqlSessionFactoryBuilder().build(Resources.getResourceAsStream("mybatis-config.xml"));
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

    private SqlSessionUtil() {
    }

    private static ThreadLocal<SqlSession> local = new ThreadLocal<>();

    /**
     * 每调用一次 openSession() 可获取一个新的会话，该会话支持自动提交
     * @return 新的会话对象
     */
    public static SqlSession openSession() {
        SqlSession sqlSession = local.get();
        if (sqlSession == null) {
            sqlSession = sqlSessionFactory.openSession();
            local.set(sqlSession);
        }
        return sqlSession;
    }

    /**
     * 关闭 SqlSession 对象
     * @param sqlSession
     */
    public static void close(SqlSession sqlSession) {
        if (sqlSession != null) {
            sqlSession.close();
        }
        local.remove();
    }
}
```



修改 dao 中的方法：AccountDaoImpl 所有方法中的 commit 和 close 全部删除

```java
public class AccountDaoImpl implements AccountDao {
    @Override
    public Account selectByActno(String actno) {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        Account act = (Account) sqlSession.selectOne("selectByActno", actno);
        return act;
    }

    @Override
    public int update(Account act) {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        int count = sqlSession.update("update", act);
        return count;
    }
}
```



修改 service 中的方法

```java
public class AccountServiceImpl implements AccountService {
    AccountDao accountDao = new AccountDaoImpl();

    @Override
    public void transfer(String fromActno, String toActno, double money)
            throws MoneyNotEnoughException, AppException {
        Account fromAct = accountDao.selectByActno(fromActno);
        if (fromAct.getBalance() < money) {
            throw new MoneyNotEnoughException("对不起，您的余额不足");
        }
        try {
            Account toAct = accountDao.selectByActno(toActno);
            fromAct.setBalance(fromAct.getBalance() - money);
            toAct.setBalance(toAct.getBalance() + money);
            // 更新数据库（添加事务）
            SqlSession sqlSession = SqlSessionUtil.openSession();
            accountDao.update(fromAct);
            // 模拟异常
            String s = null;
            s.toString();
            accountDao.update(toAct);
            sqlSession.commit();
            SqlSessionUtil.close(sqlSession); // 只修改了这一行代码
        } catch (Exception e) {
            throw new AppException("转账失败，未知原因！请联系管理员！");
        }
    }
}
```





## 5.5 MyBatis 核心对象作用域

### SqlSessionFactoryBuilder

这个类可以被实例化、使用和丢弃，一旦创建了 SqlSessionFactory，就不再需要它了。 因此 SqlSessionFactoryBuilder 实例的最佳作用域是方法作用域（也就是局部方法变量）。 你可以重用 SqlSessionFactoryBuilder 来创建多个 SqlSessionFactory 实例，但最好还是不要一直保留着它，以保证所有的 XML 解析资源可以被释放给更重要的事情。

### SqlSessionFactory

SqlSessionFactory 一旦被创建就应该在应用的运行期间一直存在，没有任何理由丢弃它或重新创建另一个实例。 使用 SqlSessionFactory 的最佳实践是在应用运行期间不要重复创建多次，多次重建 SqlSessionFactory 被视为一种代码“坏习惯”。因此 SqlSessionFactory 的最佳作用域是应用作用域。 有很多方法可以做到，最简单的就是使用单例模式或者静态单例模式。

### SqlSession

每个线程都应该有它自己的 SqlSession 实例。SqlSession 的实例不是线程安全的，因此是不能被共享的，所以它的最佳的作用域是请求或方法作用域。 绝对不能将 SqlSession 实例的引用放在一个类的静态域，甚至一个类的实例变量也不行。 也绝不能将 SqlSession 实例的引用放在任何类型的托管作用域中，比如 Servlet 框架中的 HttpSession。 如果你现在正在使用一种 Web 框架，考虑将 SqlSession 放在一个和 HTTP 请求相似的作用域中。 换句话说，每次收到 HTTP 请求，就可以打开一个 SqlSession，返回一个响应后，就关闭它。 这个关闭操作很重要，为了确保每次都能执行关闭操作，你应该把这个关闭操作放到 finally 块中。下面的示例就是一个确保 SqlSession 关闭的标准模式：

```java
try (SqlSession session = sqlSessionFactory.openSession()) {
  // 你的应用逻辑代码
}
```



##  5.6 分析当前程序存在的问题

分析 AccountDaoImpl 代码

```java
public class AccountDaoImpl implements AccountDao {
    @Override
    public Account selectByActno(String actno) {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        Account act = (Account) sqlSession.selectOne("selectByActno", actno);
        return act;
    }

    @Override
    public int update(Account act) {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        int count = sqlSession.update("update", act);
        return count;
    }
}
```

我们不难发现，这个 dao 实现类中的方法代码很固定，基本上就是一行代码，通过 SqlSession 对象调用 insert、delete、update、select 等方法，这个类中的方法没有任何业务逻辑。后边我们会学习动态生成该类，就不用再写这个类了







# 六、使用 Javassist 生成类

引入依赖

```xml
<dependency>
    <groupId>org.javassist</groupId>
    <artifactId>javassist</artifactId>
    <version>3.29.2-GA</version>
</dependency>
```



测试代码

```java
public class JavassistTest {
    public static void main(String[] args) throws Exception {
        // 获取类池
        ClassPool pool = ClassPool.getDefault();
        // 创建类
        CtClass ctClass = pool.makeClass("com.shameyang.javassist.Test");
        // 创建方法
        // 1.返回值类型 2.方法名 3.形式参数列表 4.所属类
        CtMethod ctMethod = new CtMethod(CtClass.voidType, "execute", new CtClass[]{}, ctClass);
        // 设置方法的修饰符列表
        ctMethod.setModifiers(Modifier.PUBLIC);
        // 设置方法体
        ctMethod.setBody("{System.out.println(\"hello world\");}");
        // 给类添加方法
        ctClass.addMethod(ctMethod);
        
        // 调用方法
        Class<?> aClass = ctClass.toClass();
        Object o = aClass.newInstance();
        Method method = aClass.getDeclaredMethod("execute");
        method.invoke(o);
    }
}
```

运行注意：需要加两个参数，否则会有异常

- --add-opens java.base/java.lang=ALL-UNNAMED

- --add-opens java.base/sun.net.util=ALL-UNNAMED

  <img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/javassist%E5%8F%82%E6%95%B0%E8%AE%BE%E7%BD%AE.png" style="zoom:67%;" />







# 七、MyBatis 接口代理机制及使用

> MyBatis 提供了接口代理机制，可以动态为我们生成 dao 接口的实现类（代理类：dao 接口的代理）
>
> 代理模式：在内存中生成 dao 接口的代理类，然后创建代理类的实例
>
> 前提：SqlMapper.xml 文件中
>
> - namespace 必须是 dao 接口的全限定名称
> - id 必须是 dao 接口中的方法名



使用 mybatis 获取 dao 接口代理类对象：

```java
AccountDao accountDao = (AccountDao) SqlSessionUtil.openSession().getMapper(AccountDao.class);
```



示例代码：

- 删除 AccountDaoImpl

- AccountServiceImpl 获取 dao 接口代理类对象

  ```java
  public class AccountServiceImpl implements AccountService {
      // 只有这一行进行了修改，获取 dao 接口代理类对象
      private AccountDao accountDao = (AccountDao) SqlSessionUtil.openSession().getMapper(AccountDao.class);
  
      @Override
      public void transfer(String fromActno, String toActno, double money)
              throws MoneyNotEnoughException, AppException {
          Account fromAct = accountDao.selectByActno(fromActno);
          if (fromAct.getBalance() < money) {
              throw new MoneyNotEnoughException("对不起，您的余额不足");
          }
          try {
              Account toAct = accountDao.selectByActno(toActno);
              fromAct.setBalance(fromAct.getBalance() - money);
              toAct.setBalance(toAct.getBalance() + money);
              // 更新数据库（添加事务）
              SqlSession sqlSession = SqlSessionUtil.openSession();
              accountDao.update(fromAct);
              accountDao.update(toAct);
              sqlSession.commit();
              SqlSessionUtil.close(sqlSession);
          } catch (Exception e) {
              throw new AppException("转账失败，未知原因！请联系管理员！");
          }
      }
  }
  ```

- AccountMapper 修改 namespace

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <!-- 修改了 namespace -->
  <mapper namespace="com.shameyang.bank.dao.AccountDao">
      <select id="selectByActno" resultType="com.shameyang.bank.bean.Account">
          select * from t_act where actno= #{actno};
      </select>
      <update id="update">
          update t_act set balance = #{balance} where actno = #{actno};
      </update>
  </mapper>
  ```








# 八、MyBatis 小技巧

## 8.1 #{}和${}

\#{}：先编译sql语句，再给占位符传值，底层是PreparedStatement实现。可以防止sql注入，比较常用。

${}：先进行sql语句拼接，然后再编译sql语句，底层是Statement实现。存在sql注入现象。只有在需要进行sql语句关键字拼接的情况下才会用到。



## 8.2 别名机制

在 Mapper.xml 文件中，resultType 属性用来指定查询结果集的封装类型，这个名字太长，我们可以起别名

在 mybatis-config.xml 文件中使用 \<typeAliases> 起别名（不区分大小写）

- 第一种方式：typeAlias

  ```xml
  <typeAliases>
    <typeAlias type="com.powernode.mybatis.pojo.Car" alias="Car"/>
  </typeAliases>
  ```

  

- 第二种方式：package

  如果一个包下的类太多，每个类都要起别名，会导致 typeAlias 标签配置较多，所以 mybatis 用提供 package 的配置方式，只需要指定包名，该包下的所有类都自动起别名，别名就是简类名。并且别名不区分大小写。

  ```xml
  <typeAliases>
    <package name="com.powernode.mybatis.pojo"/>
  </typeAliases>
  ```



## 8.3 mappers

SQL 映射文件的配置方式包括四种：

- resource：从类路径中加载
- url：从指定的全限定资源路径中加载
- class：使用映射器接口实现类的全限定类名、
- package：将包内的映射器接口实现全部注册为映射器



### resource

这种方式是从类路径中加载配置文件，所以这种方式要求 SQL 映射文件必须放在 resources 目录下或其子目录下

```xml
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/>
  <mapper resource="org/mybatis/builder/BlogMapper.xml"/>
  <mapper resource="org/mybatis/builder/PostMapper.xml"/>
</mappers>
```

### url

这种方式使用了绝对路径的方式，这种配置对 SQL 映射文件存放的位置没有要求

```xml
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
  <mapper url="file:///var/mappers/BlogMapper.xml"/>
  <mapper url="file:///var/mappers/PostMapper.xml"/>
</mappers>
```

### class

如果使用这种方式必须满足以下条件：

- SQL 映射文件和 mapper 接口放在同一个目录下

  ```
  src
   |---java
   	  |---com.shameyang.mybatis.mapper
   	  		|---mapper 接口
   |---resources
   	  |---com/shameyang/mybatis/mapper
   	  		|---SQL 映射文件
  ```

- SQL 映射文件的名字也必须和 mapper 接口名一致



mybatis-config.xml 文件如下

```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="com.shameyang.mybatis.mapper.AuthorMapper"/>
  <mapper class="com.shameyang.mybatis.mapper.BlogMapper"/>
  <mapper class="com.shameyang.mybatis.mapper.PostMapper"/>
</mappers>
```

### package

如果class较多，可以使用这种package的方式，但前提条件和上一种方式一样

```xml
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="com.powernode.mybatis.mapper"/>
</mappers>
```



## 8.4 IDEA 配置文件模板

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6%E6%A8%A1%E6%9D%BF.png" style="zoom:67%;float:left" />



## 8.5 插入数据时获取自动生成的主键

XxxMapper 接口中

```java
void insertUseGeneratedKeys(Xxx xxx);
```

XxxMapper.xml

``` xml
<!--
	useGenerateKeys：是否使用自动生成的主键
	keyProperty：指定主键值赋给对象的哪个属性
-->
<insert id="insertUseGeneratedKeys" useGeneratedKeys="true" keyProperty="id">
  ...
</insert>
```







# 九、MyBatis 参数处理

## 9.1 单个简单类型参数

简单类型包括：

- byte short int long float double char
- Byte Short Integer Long Float Double Character
- String
- java.util.Date
- java.sql.Date



简单类型对于 mybatis 来说都是可以自动识别类型的

SQL映射文件中的配置比较完整的写法是：

```xml
<select id="selectByName" resultType="student" parameterType="java.lang.String">
  select * from t_student where name = #{name, javaType=String, jdbcType=VARCHAR}
</select>
```

其中 sql 语句中的 javaType，jdbcType，以及 select 标签中的 parameterType 属性，都是用来帮助 mybatis 进行类型确定的。不过这些配置多数是可以省略的，因为 mybatis 有强大的自动类型推断机制



## 9.2 Map 参数

StudentMapper 接口

```java
/**
* 根据name和age查询
* @param paramMap
* @return
*/
List<Student> selectByParamMap(Map<String, Object> paramMap);
```

StudentMapper.xml

```xml
<select id="selectByParamMap" resultType="student">
  select * from t_student where name = #{nameKey} and age = #{ageKey}
</select>
```

StudentMapperTest

```java
@Test
public void testSelectByParamMap(){
    // 准备Map
    Map<String, Object> paramMap = new HashMap<>();
    paramMap.put("nameKey", "张三");
    paramMap.put("ageKey", 20);

    List<Student> students = mapper.selectByParamMap(paramMap);
    students.forEach(student -> System.out.println(student));
}
```



**这种方式是手动封装 Map 集合，将每个条件以 key 和 value 的形式存放到集合中。然后在使用的时候通过#{map 集合的 key}来取值**



## 9.3 实体类参数

StudentMapper 接口

```xml
/**
 * 保存学生数据
 * @param student
 * @return
 */
int insert(Student student);
```

StudentMapper.xml

```xml
<insert id="insert">
  insert into t_student values(null,#{name},#{age},#{height},#{birth},#{sex})
</insert>
```

StudentMapperTest

```java
@Test
public void testInsert(){
    Student student = new Student();
    student.setName("李四");
    student.setAge(30);
    student.setHeight(1.70);
    student.setSex('男');
    student.setBirth(new Date());
    int count = mapper.insert(student);
    SqlSessionUtil.openSession().commit();
}
```



**这里需要注意的是：#{} 里面写的是属性名字。这个属性名其本质上是：set/get方法名去掉set/get之后的名字**



## 9.4 多参数

StudentMapper 接口

```java
/**
 * 根据name和sex查询
 * @param name
 * @param sex
 * @return
 */
List<Student> selectByNameAndSex(String name, Character sex);
```

StudentMapper.xml

```xml
<select id="selectByNameAndSex" resultType="student">
  select * from t_student where name = #{name} and sex = #{sex}
</select>
```

StudentMapperTest

```java
@Test
public void testSelectByNameAndSex(){
    List<Student> students = mapper.selectByNameAndSex("张三", '女');
    students.forEach(student -> System.out.println(student));
}
```

执行结果：

![img](https://cdn.nlark.com/yuque/0/2022/png/21376908/1660641021618-ce3ac913-fe10-45f5-9760-3e51ef2dd864.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_47%2Ctext_5Yqo5Yqb6IqC54K5%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10)



异常信息描述了：name 参数找不到，可用的参数包括[arg1, arg0, param1, param2]



修改 StudentMapper.xml 配置文件：尝试使用[arg1, arg0, param1, param2]去参数

```xml
<select id="selectByNameAndSex" resultType="student">
  select * from t_student where name = #{arg0} and sex = #{arg1}
</select>
```

运行结果：

<img src="https://cdn.nlark.com/yuque/0/2022/png/21376908/1660641284279-64a7312a-d036-448f-aaef-a1bcde8abba2.png?x-oss-process=image%2Fwatermark%2Ctype_d3F5LW1pY3JvaGVp%2Csize_27%2Ctext_5Yqo5Yqb6IqC54K5%2Ccolor_FFFFFF%2Cshadow_50%2Ct_80%2Cg_se%2Cx_10%2Cy_10" alt="img" style="zoom: 80%;float:left" />



再次尝试修改 StudentMapper.xml 文件

```xml
<select id="selectByNameAndSex" resultType="student">
  select * from t_student where name = #{arg0} and sex = #{param2}
</select>
```

通过测试可以看到：

- arg0 是第一个参数
- param1 是第一个参数
- arg1 是第二个参数
- param2 是第二个参数



实现原理：**实际上在 mybatis 底层会创建一个 map 集合，以 arg0/param1为 key，以方法上的参数为 value**

例如以下代码：mybatis 部分源码

```java
Map<String,Object> map = new HashMap<>();
map.put("arg0", name);
map.put("arg1", sex);
map.put("param1", name);
map.put("param2", sex);

// 所以可以这样取值：#{arg0} #{arg1} #{param1} #{param2}
// 其本质就是#{map集合的key}
```



注意：使用mybatis3.4.2之前的版本时：要用#{0}和#{1}这种形式



## 9.5 @Param 注解（命名参数）

使用 @Param 注解在多参数时，就不需要使用 arg 或 param 参数了，增强了可读性



@Param 使用

```java
/**
 * 根据name和age查询
 * @param name
 * @param age
 * @return
 */
List<Student> selectByNameAndAge(@Param("name") String name, @Param("age") int age);
```

```java
@Test
public void testSelectByNameAndAge(){
    List<Student> stus = mapper.selectByNameAndAge("张三", 20);
    stus.forEach(student -> System.out.println(student));
}
```

```xml
<select id="selectByNameAndAge" resultType="student">
  select * from t_student where name = #{name} and age = #{age}
</select>
```

原理：@Param(key) 中的参数就是 map 集合的 key
