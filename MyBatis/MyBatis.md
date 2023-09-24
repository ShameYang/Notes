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

> JDBC 代码中占位符采用的是 ?，在 MyBatis 中是 #{ }



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
