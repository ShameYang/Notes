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

更多别名参考 MyBatis 手册

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







# 十、MyBatis 查询专题

## 10.1 resultMap 结果映射

查询结果的列名和 java 对象的属性名对应不上怎么办？

- as 给列起别名
- 使用 resultMap 进行结果映射
- 是否开启驼峰命名自动映射（配置 settings）



### 使用 resultMap 进行结果映射

> 当属性名和数据库列名一致时，可以省略。但建议都写上

```xml
<!--
	id：这个结果映射的标识，作为 select 标签的 resultMap 属性的值。
	type：结果集要映射的类。可以使用别名。
-->
<resultMap id="carResultMap" type="car">
	<!-- 对象的唯一标识，官方解释是：为了提高mybatis的性能（建议写上）-->
	<id property="id" column="id"/>
	<result property="carNum" column="car_num"/>
	<!-- javaType 用来指定属性类型。jdbcType 用来指定列类型。一般可以省略。-->
	<result property="brand" column="brand" javaType="string" jdbcType="VARCHAR"/>
	<result property="guidePrice" column="guide_price"/>
	<result property="produceTime" column="produce_time"/>
	<result property="carType" column="car_type"/>
</resultMap>

<!--resultMap 属性的值必须和 resultMap 标签中 id 属性值一致。-->
<select id="selectAllByResultMap" resultMap="carResultMap">
	select * from t_car
</select>
```



### 是否开启驼峰命名自动映射

使用前提：属性名遵循 Java 的命名规范，数据库表的列名遵循 SQL 的命名规范

Java 命名规范：首字母小写，后面每个单词首字母大写，遵循驼峰命名方式

SQL 命名规范：全部小写，单词之间采用下划线分割



比如以下对应关系

| 实体类中的属性名 | 数据库表中的列名 |
| ---------------- | ---------------- |
| stuNum           | stu_num          |
| stuName          | stu_name         |



启用该功能：配置 mybatis-config.xml

```xml
<settings>
    <setting name="mapUnderscoreToCamelCase" value="true"/>
</settings>
```



## 10.2 返回总记录条数

```java
/**
* 获取总记录条数
* @return
*/
Long selectTotal();
```

```xml
<!--long是别名，可参考mybatis开发手册。-->
<select id="selectTotal" resultType="long">
	select count(*) from t_car
</select>
```

```java
@Test
public void testSelectTotal(){
    CarMapper carMapper = SqlSessionUtil.openSession().getMapper(CarMapper.class);
    Long total = carMapper.selectTotal();
    System.out.println(total);
}
```







# 十一、动态 SQL

顾名思义，动态 SQL 可以满足需要 SQL 语句拼接的业务场景，例如：批量删除、多条件查询。极大的便利了我们编写 SQL 语句的拼接



## 11.1 if

把 where 子句放到 if 标签中

```xml
<select id="findActiveBlogWithTitleLike" resultType="Blog">
	SELECT * FROM BLOG WHERE state = ‘ACTIVE’
	<if test="title != null">
		AND title like #{title}
	</if>
</select>
```

这条语句提供了可选的查找文本功能。如果不传入 “title”，那么所有处于 “ACTIVE” 状态的 BLOG 都会返回；如果传入了 “title” 参数，那么就会对 “title” 一列进行模糊查找并返回对应的 BLOG 结果



如果希望通过 “title” 和 “author” 两个参数进行可选搜索怎么办？只需要再加入一个条件即可

```xml
<select id="findActiveBlogLike" resultType="Blog">
	SELECT * FROM BLOG WHERE state = ‘ACTIVE’
	<if test="title != null">
		AND title like #{title}
	</if>
	<if test="author != null and author.name != null">
		AND author_name like #{author.name}
	</if>
</select>
```



## 11.2 choose、when、otherwise

有时候，我们不想使用所有的条件，而只是想从多个条件中选择一个使用。针对这种情况，MyBatis 提供了 choose 元素，它有点像 Java 中的 switch 语句

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG WHERE state = ‘ACTIVE’
	<choose>
		<when test="title != null">
			AND title like #{title}
		</when>
		<when test="author != null and author.name != null">
			AND author_name like #{author.name}
		</when>
		<otherwise>
			AND featured = 1
		</otherwise>
	</choose>
</select>
```



## 11.3 where、trim、set

使用 where 标签，就不要在 SQL 中写 where 关键字了。如果子句的开头是 and 或 or，where 会自动去除，我们编写就更加灵活了

示例如下：

```xml
<select id="findActiveBlogLike" resultType="Blog">
  SELECT * FROM BLOG
	<where>
		<if test="state != null">
		     state = #{state}
		</if>
		<if test="title != null">
		    AND title like #{title}
		</if>
		<if test="author != null and author.name != null">
		    AND author_name like #{author.name}
		</if>
	</where>
</select>
```



我们还可以通过 trim 标签来定制 where 元素的功能，例如与 where 元素等价的自定义 trim 元素如下：

```xml
<!-- 
	prefix：添加前缀
	suffix：添加后缀
	prefixOverrides：前缀覆盖掉（去掉）
	suffixOverrides：后缀覆盖掉（去掉）
-->
<trim prefix="WHERE" prefixOverrides="AND |OR ">
	...
</trim>
```



set 标签用于动态更新语句

```xml
<update id="updateAuthorIfNecessary">
	update Author
	  <set>
	  	<if test="username != null">username=#{username},</if>
	  	<if test="password != null">password=#{password},</if>
	  	<if test="email != null">email=#{email},</if>
	  	<if test="bio != null">bio=#{bio}</if>
	  </set>
	where id=#{id}
</update>
```

这个例子中，set 元素会动态地在行首插入 SET 关键字，并会删掉额外的逗号（这些逗号是在使用条件语句给列赋值时引入的）



与 set 等价的 trim 元素如下：

```xml
<trim prefix="SET" suffixOverrides=",">
	...
</trim>
```



## 11.4 foreach

```xml
<select id="selectPostIn" resultType="domain.blog.Post">
	SELECT * FROM post p WHERE id in
    <!--
		collection: 集合或数组
		item: 本次迭代获取到的元素（集合或数组中的元素）
		index: 当前迭代的序号
		open: foreach 标签中所有内容的开始
		separator: 分隔符
		close: foreach 标签中所有内容的结束
	-->
	<foreach collection="list" item="item" index="index" open="(" separator="," close=")">
		#{item}
	</foreach>
</select>
```



## 11.5 sql 标签和 include 标签

sql 标签用来声明 sql 片段

include 标签用来将声明的 sql 片段包含到某个 sql 语句中

作用：代码复用，易维护

```xml
<sql id="multiplexCode">
	复用的 sql 片段
</sql>

<select id="selectAllRetMap" resultType="map">
	select <include refid="multiplexCode"/> from xxx
</select>

<select id="selectAllRetListMap" resultType="map">
	select <include refid="multiplexCode"/> xxx from xxx
</select>

<select id="selectByIdRetMap" resultType="map">
	select <include refid="multiplexCode"/> from xxx where ...
</select>
```







# 十二、高级映射及延迟加载

在多表中，谁是主表，谁就是 JVM 中的主对象，例如学生班级表（多对一），学生表是主表，Student 对象就是主对象

## 12.1 多对一

常见的三种方式：

- 一条 SQL 语句，级联属性映射
- 一条 SQL 语句，association
- 两条 SQL 语句，分步查询（比较常用：优点是可复用，而且支持懒加载）



在主表对应的类中，添加关联的副表对象

```java
public class Student {
    private Integer sid;
    private String sname;
    private Clazz clazz;
    // set get方法
    // 构造方法
    // toString方法
}
```



### 第一种方式：级联属性映射

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
	PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.shameyang.mybatis.mapper.StudentMapper">

    <resultMap id="studentResultMap" type="Student">
        <id property="sid" column="sid"/>
        <result property="sname" column="sname"/>
        <!-- 级联属性映射 -->
        <result property="clazz.cid" column="cid"/>
        <result property="clazz.cname" column="cname"/>
    </resultMap>

    <select id="selectBySid" resultMap="studentResultMap">
        select 
        	s.*, c.* 
        from 
        	t_student s join t_clazz c 
        on 
        	s.cid = c.cid 
        where 
        	sid = #{sid}
    </select>

</mapper>
```



### 第二种方式：association

其他位置不需要修改，只需要修改 resultMap 中的配置即可

```xml
<resultMap id="studentResultMap" type="Student">
	<id property="sid" column="sid"/>
	<result property="sname" column="sname"/>
	<!-- 关联 Clazz 类 -->
	<association property="clazz" javaType="Clazz">
		<id property="cid" column="cid"/>
		<result property="cname" column="cname"/>
	</association>
</resultMap>
```



### 第三种方式：分步查询

第一步：修改 resultMap 中 association 的属性

```xml
<resultMap id="studentResultMap" type="Student">
	<id property="sid" column="sid"/>
	<result property="sname" column="sname"/>
    <!-- select 指定第二步查询的方法 -->
	<association property="clazz"
               select="com.shameyang.mybatis.mapper.ClazzMapper.selectByCid"
               column="cid"/>
</resultMap>

<select id="selectBySid" resultMap="studentResultMap">
	select 
    	sid, sname, cid 
    from 
    	t_student s 
    where 
    	sid = #{sid}
</select>
```

第二步：ClazzMapper 接口中添加方法

```java
public interface ClazzMapper {

    /**
     * 根据 cid 获取 Clazz 信息
     * @param cid
     * @return
     */
    Clazz selectByCid(Integer cid);
}
```

第三步：配置 ClazzMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.shameyang.mybatis.mapper.ClazzMapper">
    <select id="selectByCid" resultType="Clazz">
        select 
        	cid, cname 
        from 
        	t_clazz 
        where 
        	cid = #{cid}
    </select>
</mapper>
```



## 12.2 多对一延迟加载

延迟加载即暂时不访问的数据先不查询，提高程序的执行效率

延迟加载的实现很简单：在 association 标签中添加 fetchType="lazy" 即可实现局部延迟加载

```xml
<resultMap id="studentResultMap" type="Student">
	<id property="sid" column="sid"/>
	<result property="sname" column="sname"/>
	<association property="clazz"
	             select="com.shameyang.mybatis.mapper.ClazzMapper.selectByCid"
	             column="cid"
	             fetchType="lazy"/>
</resultMap>
```

只有当使用到 cid 时，才会执行关联的语句



上面的例子是局部延迟加载，我们还可以通过配置 mybatis-config.xml 进行全局设置

```xml
<settings>
  <setting name="lazyLoadingEnabled" value="true"/>
</settings>
```



开启全局延迟加载后，如果不希望某个 sql 延迟加载，将 fetchType 设置为 eager 即可：

```xml
<resultMap id="studentResultMap" type="Student">
	<id property="sid" column="sid"/>
	<result property="sname" column="sname"/>
	<association property="clazz"
	             select="com.shameyang.mybatis.mapper.ClazzMapper.selectByCid"
	             column="cid"
	             fetchType="eager"/>
</resultMap>
```



## 12.3 一对多

一对多，通常在一的一方中有 List 集合属性

```java
public class Clazz {
    private Integer cid;
    private String cname;
    private List<Student> stus;
    // set get方法
    // 构造方法
    // toString方法
}
```



常见的两种实现方式：

- collection
- 分步查询



### 第一种方式：collection

```xml
<resultMap id="clazzResultMap" type="Clazz">
	<id property="cid" column="cid"/>
	<result property="cname" column="cname"/>
    <!-- 注意：是 ofType，表示集合中的类型 -->
	<collection property="stus" ofType="Student">
		<id property="sid" column="sid"/>
		<result property="sname" column="sname"/>
	</collection>
</resultMap>

<select id="selectClazzAndStusByCid" resultMap="clazzResultMap">
	select * from t_clazz c join t_student s on c.cid = s.cid where c.cid = #{cid}
</select>
```



### 第二种方式：分步查询

与多对一同理，只是 association 换成了 collection

第一步：配置 collection 标签

```xml
<resultMap id="clazzResultMap" type="Clazz">
	<id property="cid" column="cid"/>
	<result property="cname" column="cname"/>
	<!--主要看这里-->
	<collection property="stus"
	            select="com.shameyang.mybatis.mapper.StudentMapper.selectByCid"
	            column="cid"/>
</resultMap>

<!--sql语句也变化了-->
<select id="selectClazzAndStusByCid" resultMap="clazzResultMap">
	select * from t_clazz c where c.cid = #{cid}
</select>
```

第二步：StudentMapper 接口中添加方法

```java
/**
* 根据班级编号获取所有的学生。
* @param cid
* @return
*/
List<Student> selectByCid(Integer cid);
```

第三步：配置 StudentMapper.xml

```xml
<select id="selectByCid" resultType="Student">
	select * from t_student where cid = #{cid}
</select>
```



## 12.4 一对多延迟加载

一对多的延迟加载与[多对一延迟加载](##12.2 多对一延迟加载)一样







# 十三、MyBatis 的缓存

缓存：cache

缓存的作用：通过减少 IO 的方式，来提高程序的执行效率

mybatis 的缓存：将 select 语句的查询结果放到缓存（内存）当中，下一次还是这条 select 语句的话，直接从缓存中取，不再查数据库。一方面是减少了 IO。另一方面不再执行繁琐的查找算法。效率大大提升

mybatis 缓存包括：

- 一级缓存：将查询到的数据存储到 SqlSession 中
- 二级缓存：将查询到的数据存储到 SqlSessionFactory 中
- 或者集成其它第三方的缓存：比如 EhCache【Java语言开发的】、Memcache【C语言开发的】等

**缓存只针对于DQL语句，也就是说缓存机制只对应select语句。**

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E7%BC%93%E5%AD%98%E7%9A%84%E7%90%86%E8%A7%A3.png)



## 13.1 一级缓存

一级缓存默认开启，不需要配置

原理：只要使用同一个 SqlSession 对象执行同一条 SQL 语句，就会走缓存



一级缓存失效的情况：

- 两次查询之间，手动清空了一级缓存

  ```java
  sqlSession.clearCache();
  ```

- 两次查询之间，执行了增删改操作



## 13.2 二级缓存

默认是开启二级缓存的

```xml
<!-- 默认为 true -->
<setting name="cacheEnabled" value="true">
```



使用二级缓存，只需要在 SQL 的映射文件中添加一行：

```xml
<cache/>
```

注意：

- 使用二级缓存的实体类对象必须是可序列化的，即必须实现 java.io.Serializable 接口

- SqlSession 对象关闭或提交之后，一级缓存中的数据才会被写入到二级缓存当中。此时二级缓存才可用



**二级缓存的相关配置：**

- eviction：指定从缓存中移除某个对象的淘汰算法。默认采用LRU策略
  - LRU：Least Recently Used。最近最少使用。优先淘汰在间隔时间内使用频率最低的对象。(其实还有一种淘汰算法LFU，最不常用)
  - FIFO：First In First Out。一种先进先出的数据缓存器。先进入二级缓存的对象最先被淘汰
  - SOFT：软引用。淘汰软引用指向的对象。具体算法和JVM的垃圾回收算法有关
  - WEAK：弱引用。淘汰弱引用指向的对象。具体算法和JVM的垃圾回收算法有关

- flushInterval：
  
  二级缓存的刷新时间间隔。单位毫秒。如果没有设置。就代表不刷新缓存，只要内存足够大，一直会向二级缓存中缓存数据。除非执行了增删改
  
- readOnly：
  - true：多条相同的sql语句执行之后返回的对象是共享的同一个。性能好。但是多线程并发可能会存在安全问题
  - false：多条相同的sql语句执行之后返回的对象是副本，调用了clone方法。性能一般。但安全

- size：

  设置二级缓存中最多可存储的java对象数量。默认值1024



## 13.3 MyBatis 集成 EhCache

我们可以集成第三方缓存，这里以 EhCache 为例：修改 cache 标签的属性即可

第一步：引入 ehcache 依赖

```xml
<!-- mybatis 集成 ehcache 的组件 -->
<dependency>
  <groupId>org.mybatis.caches</groupId>
  <artifactId>mybatis-ehcache</artifactId>
  <version>1.2.2</version>
</dependency>
```

第二步：在类的根路径下新建 ehcache.xml 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
         updateCheck="false">
    <!--磁盘存储:将缓存中暂时不使用的对象,转移到硬盘,类似于Windows系统的虚拟内存-->
    <diskStore path="e:/ehcache"/>
  
    <!--defaultCache：默认的管理策略-->
    <!--eternal：设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断-->
    <!--maxElementsInMemory：在内存中缓存的element的最大数目-->
    <!--overflowToDisk：如果内存中数据超过内存限制，是否要缓存到磁盘上-->
    <!--diskPersistent：是否在磁盘上持久化。指重启jvm后，数据是否有效。默认为false-->
    <!--timeToIdleSeconds：对象空闲时间(单位：秒)，指对象在多长时间没有被访问就会失效。只对eternal为false的有效。默认值0，表示一直可以访问-->
    <!--timeToLiveSeconds：对象存活时间(单位：秒)，指对象从创建到失效所需要的时间。只对eternal为false的有效。默认值0，表示一直可以访问-->
    <!--memoryStoreEvictionPolicy：缓存的3 种清空策略-->
    <!--FIFO：first in first out (先进先出)-->
    <!--LFU：Less Frequently Used (最少使用).意思是一直以来最少被使用的。缓存的元素有一个hit 属性，hit 值最小的将会被清出缓存-->
    <!--LRU：Least Recently Used(最近最少使用). (ehcache 默认值).缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存-->
    <defaultCache eternal="false" maxElementsInMemory="1000" overflowToDisk="false" diskPersistent="false"
                  timeToIdleSeconds="0" timeToLiveSeconds="600" memoryStoreEvictionPolicy="LRU"/>

</ehcache>
```

第三步：修改 SqlMapper.xml 文件中的 cache 标签，添加 type 属性

```xml
<cache type="org.mybatis.caches.ehcache.EhcacheCache"/>
```







# 十四、MyBatis 的逆向工程

逆向工程就是：根据数据库表逆向生成 Java 的 pojo 类，SqlMapper.xml 以及 Mapper 接口等

我们可以借助大佬写好的逆向工程插件，完成逆向工程



## 14.1 逆向工程配置与生成

第一步：基础环境配置

- 新建 maven 模块

- 打包方式：jar



第二步：pom 文件中添加逆向工程插件

```xml
<!--定制构建过程-->
<build>
    <!--可配置多个插件-->
    <plugins>
        <!--其中的一个插件：mybatis逆向工程插件-->
        <plugin>
            <!--插件的GAV坐标-->
            <groupId>org.mybatis.generator</groupId>
            <artifactId>mybatis-generator-maven-plugin</artifactId>
            <version>1.4.1</version>
            <!--允许覆盖-->
            <configuration>
                <overwrite>true</overwrite>
            </configuration>
            <!--插件的依赖-->
            <dependencies>
                <!--mysql驱动依赖-->
                <dependency>
                    <groupId>com.mysql</groupId>
                    <artifactId>mysql-connector-j</artifactId>
                    <version>8.0.33</version>
                </dependency>
            </dependencies>
        </plugin>
    </plugins>
</build>
```



第三步：配置 generatorConfig.xml

- 文件名必须叫做：generatorConfig.xml
- 该文件必须放在类的根路径下

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--
        targetRuntime有两个值：
            MyBatis3Simple：生成的是基础版，只有基本的增删改查。
            MyBatis3：生成的是增强版，除了基本的增删改查之外还有复杂的增删改查。
    -->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!--防止生成重复代码-->
        <plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin"/>

        <commentGenerator>
            <!--是否去掉生成日期-->
            <property name="suppressDate" value="true"/>
            <!--是否去除注释-->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <!--连接数据库信息-->
        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/formybatis"
                        userId="root"
                        password="123456">
            <!-- 解决table schema中有多个重名的表生成表结构不一致问题 -->
            <property name="nullCatalogMeansCurrent" value="true"/>
        </jdbcConnection>

        <!-- 生成pojo包名和位置 -->
        <javaModelGenerator targetPackage="com.shameyang.mybatis.pojo" targetProject="src/main/java">
            <!--是否开启子包-->
            <property name="enableSubPackages" value="true"/>
            <!--是否去除字段名的前后空白-->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>

        <!-- 生成SQL映射文件的包名和位置 -->
        <sqlMapGenerator targetPackage="com.shameyang.mybatis.mapper" targetProject="src/main/resources">
            <!--是否开启子包-->
            <property name="enableSubPackages" value="true"/>
        </sqlMapGenerator>

        <!-- 生成Mapper接口的包名和位置 -->
        <javaClientGenerator
                type="xmlMapper"
                targetPackage="com.shameyang.mybatis.mapper"
                targetProject="src/main/java">
            <property name="enableSubPackages" value="true"/>
        </javaClientGenerator>

        <!-- 表名和对应的实体类名-->
        <table tableName="t_student" domainObjectName="Student"/>

    </context>
</generatorConfiguration>
```



## 14.2 测试逆向工程（使用 QBC 风格）

QBC 风格：Query By Criteria，一种查询方式，比较面向对象，看不到 sql 语句

```java
public class GeneratorTest {
    @Test
    public void testSelectByPrimaryKey() {
        SqlSession sqlSession = SqlSessionUtil.openSession();
        StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
        // QBC 风格
        StudentExample studentExample = new StudentExample();
        studentExample.createCriteria()
                .andSnameEqualTo("Alice");
        // 添加 or 条件
        studentExample.or().andSnoBetween(2, 5);
        List<Student> students = mapper.selectByExample(studentExample);
        System.out.println(students);
        sqlSession.commit();
        sqlSession.close();
    }
}
```







# 十五、MyBatis 使用 PageHelper

## 15.1 回顾 limit 分页

MySQL 的 limit 后面两个数字：

- 第一个数字：startIndex（起始下标，下标从0开始）
- 第二个数字：pageSize（每页显示的记录条数）

已知页码 pageNum，每页显示的记录条数 pageSize

- startIndex = (pageNum - 1) * pageSize



标准通用的 MySQL 分页：

```sql
select
	* 
from
	tableName
limit
	(pageNum - 1) * pageSize, pageSize
/* 例如 pageNum = 3
   limit 6, 3 就是从第7条记录开始，每页显示3条记录 */
```



获取数据不难，但是获取分页信息（例如总页数）比较难，因此我们可以借助 PageHelper 插件



PageHelper 是 MyBatis 的一个插件，其作用是更加方便地进行分页查询



## 15.2 使用 PageHelper

第一步：引入依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.3.3</version>
</dependency>
```

第二步：在 mybatis-config.xml 文件中配置插件

```xml
<plugins>
    <plugin interceptor="com.github.pagehelper.PageHelper"/>
</plugins>
```

第三步：编写 Java 程序

- 查询语句之前开启分页功能
- 查询语句之后封装 PageInfo 对象（PageInfo 对象将来会存储到 request 域当中，在页面上展示）

```java
@Test
public void testSelectAll() {
	SqlSession sqlSession = SqlSessionUtil.openSession();
	StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
    
	int pageNum = 1;
	int pageSize = 3;
    // 开启分页
	PageHelper.startPage(pageNum, pageSize);
    
    // 执行查询语句
	List<Student> students = mapper.selectAll();
    
    // 获取分页信息对象
	PageInfo<Student> pageInfo = new PageInfo<>(students, 5);
    
	System.out.println(pageInfo);
	sqlSession.commit();
	sqlSession.close();
}
```



执行结果：
PageInfo{pageNum=1, pageSize=3, size=3, startRow=1, endRow=3, total=5, pages=2, list=Page{count=true, pageNum=1, pageSize=3, startRow=0, endRow=3, total=5, pages=2, reasonable=false, pageSizeZero=false}[Student{sno=1, sname='tom', ssex='man'}, Student{sno=2, sname='jacky', ssex='man'}, Student{sno=8, sname='jacky', ssex='man'}], prePage=0, nextPage=2, isFirstPage=true, isLastPage=false, hasPreviousPage=false, hasNextPage=true, navigatePages=5, navigateFirstPage=1, navigateLastPage=2, navigatepageNums=[1, 2]}



执行结果格式化：

```
PageInfo{
	pageNum=1, pageSize=3, size=3, startRow=1, endRow=3, total=5, pages=2, 
	list=Page{count=true, pageNum=1, pageSize=3, startRow=0, endRow=3, total=5, pages=2, reasonable=false, pageSizeZero=false}
	[Student{sno=1, sname='tom', ssex='man'}, Student{sno=2, sname='jacky', ssex='man'}, 
	Student{sno=8, sname='jacky', ssex='man'}], 
	prePage=0, nextPage=2, isFirstPage=true, isLastPage=false, hasPreviousPage=false, hasNextPage=true, 
	navigatePages=5, navigateFirstPage=1, navigateLastPage=2, navigatepageNums=[1, 2]
}
```

