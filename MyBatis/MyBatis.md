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
