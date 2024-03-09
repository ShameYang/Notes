# 一、SpringBoot3 介绍

## 1.1 SpringBoot 简介

SpringBoot 可以简化 Spring 应用的创建及部署（底层是 Spring），只需要编写少量配置即可快速整合 Spring 平台以及第三方技术



## 1.2 系统要求

| 技术&工具 | 版本                          |
| --------- | ----------------------------- |
| Maven     | 3.6.3 or later 3.6.3 或高版本 |
| Tomcat    | 10.0+                         |
| Servlet   | 9.0+                          |
| JDK       | 17+                           |







# 二、快速入门

## 2.1 案例

> 场景：浏览器发送/hello 请求，返回"Hello,SpringBoot3"

开发步骤：

1. 创建 Maven 工程
2. 添加依赖（SpringBoot 父工程依赖，Web 启动器依赖）
3. 编写启动引导类（Springboot 项目运行入口）
4. 编写处理器 Controller
5. 启动项目（运行启动类即可）

具体实现：

- 添加依赖

  ```xml
  <!--所有springboot项目都必须继承自 spring-boot-starter-parent-->
  <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>3.0.5</version>
  </parent>
  
  <dependencies>
  	<!--web开发的场景启动器-->
      <dependency>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
  </dependencies>
  ```

- 启动引导类

  ```java
  @SpringBootApplication // 启动类
  public class Main {
      public static void main(String[] args) {
          SpringApplication.run(Main.class, args); // 自动创建IoC容器，启动Tomcat服务器软件
      }
  }
  ```

- 处理器 Controller

  > IoC 和 DI 注解在启动类的同包或子包下即可生效

  ```java
  @RestController
  @RequestMapping("hello")
  public class HelloController {
      @GetMapping("boot")
      public String hello() {
          return "Hello,SpringBoot3";
      }
  }
  ```



## 2.2 总结

1. 为什么依赖不用写版本？

   父项目 spring-boot-starter-parent 的父项目 spring-boot-dependencies 中提供了所有常见的 jar 包依赖版本

2. 启动器（Starter）是什么？

   启动器是一个整合包，整合了一组相关的依赖和配置，在启动应用程序时可以自动引入所需的库、配置和功能

   [springboot 提供的全部启动器](https://docs.spring.io/spring-boot/docs/current/reference/html/using.html?spm=wolai.workspace.0.0.68b62306vOd4xO#using.build-systems.starters)

   命名规范：

   - 官方提供：spring-boot-starter-*
   - 第三方提供：*-spring-boot-starter

3. @SpringBootApplication 注解的作用？

   他是添加到启动类的一个组合注解

   ```java
   @SpringBootConfiguration
   @EnableAutoConfiguration
   @ComponentScan
   public @interface SpringBootApplication {}
   ```







# 三、SpringBoot3 配置文件

## 3.1 统一配置管理

SpringBoot 工程下，进行统一的配置管理，将配置信息集中到配置文件（`application.properties` 或 `application.yml`）中即可

配置文件放在 src/main/resources 目录下

[官方提供的参数](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties)



## 3.2 属性配置文件使用

application.properties

```properties
# 官方提供固定的key
# 例如：启动端口号
server.port=80

# 自定义
spring.jdbc.datasource.username=root
```

读取自定义的参数，使用@Value 注解即可

```java
@Component
public class DataSourceProperties {
    @Value("${spring.jdbc.datasource.username}")
    private String username;
}
```



## 3.3 YAML 配置文件使用

1. YAML 格式介绍

   YAML 是一种基于层次结构的数据序列化格式，可以提供易读的数据表示方式

   该格式相对于 `.properties` 文件，复杂的配置具有更好的可读性

2. YAML 语法

   a.数据结构用树形结构呈现，缩进表示层级

   b.连续的项目（集合）用减号 “-” 表示

   c.键值对 key/value 用冒号 ”:“ 分隔，冒号后边必须加空格！！

   d.文件扩展名为 yml 或 yaml

   例如：

   ```yaml
   # YAML配置文件示例
   app_name: 我的应用程序
   version: 1.0.0
   author: 张三
   
   database:
     host: localhost
     port: 5432
     username: admin
     password: password123
   
   features:
     - 登录
     - 注册
     - 仪表盘
   
   settings:
     analytics: true
     theme: dark
   ```

3. 读取方式与 properties 一致，使用@Value 即可



## 3.4 批量配置文件注入

之前使用@Value 只能读取单个值，我们可以使用@ConfigurationProperties 注解，将配置属性批量注入到 bean 对象

在类上添加该注解即可：

```java
@Component
@ConfigurationProperties(prefix = "spring.jdbc.datasource")
public class DataSourceConfigurationProperties {
    private String driverClassName;
    private String url;
    private String username;
    private String password;
    // ...
}
```



## 3.5 多环境配置和使用

在Spring Boot中，可以使用多环境配置来根据不同的运行环境（如开发、测试、生产）加载不同的配置

以 yaml 文件为例：

1. 多环境配置

   > 创建开发、测试、生产三个环境的配置文件

   application-dev.yml（开发）

   ```yaml
   spring:
     jdbc:
       datasource:
         driverClassName: com.mysql.cj.jdbc.Driver
         url: jdbc:mysql:///dev
         username: root
         password: root
   ```

   application-test.yml（测试）

   ```yaml
   spring:
     jdbc:
       datasource:
         driverClassName: com.mysql.cj.jdbc.Driver
         url: jdbc:mysql:///test
         username: root
         password: root
   ```

   application-prod.yml（生产）

   ```yaml
   spring:
     jdbc:
       datasource:
         driverClassName: com.mysql.cj.jdbc.Driver
         url: jdbc:mysql:///prod
         username: root
         password: root
   ```

2. 环境激活

   application.yml

   ```yaml
   spring:
     profiles:
       active: dev # 多个用逗号隔开
   ```

   注意：如果设置了 spring.profiles.active，并且和 application 有重叠属性，以 active 设置的属性优先







# 四、SpringBoot3 整合 SpringMVC

## 4.1 实现过程

第一步：pom.xml 添加依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.0.5</version>
    </parent>

    <groupId>com.shameyanng</groupId>
    <artifactId>springboot-002-springmvc</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
</project>
```

第二步：编写启动类

```java
@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }
}
```

第三步：编写 Controller

第四步：访问测试



## 4.2 web 相关配置

位置：application.yml

```yaml
# web相关的配置
server:
  # 端口号设置
  port: 80
  # 项目根路径
  servlet:
    context-path: /boot
```

五个常用配置参数：

- server.port

  HTTP 服务器端口号，SpringBoot 默认使用 8080

- server.servlet.context-path

  设置应用程序的上下文路径

- spring.mvc.view.prefix 和 spring.mvc.view.suffix

  配置视图解析器的前缀和后缀

- spring.resources.static-locations

  配置静态资源的位置

- spring.http.encoding.charset 和 spring.http.encoding.enabled

  配置 HTTP 请求和响应的字符编码

根据需求在配置文件中设置这些参数，更多参数查阅[官方文档](https://docs.spring.io/spring-boot/docs/current/reference/html/application-properties.html#appendix.application-properties.server)



## 4.3 静态资源处理

> SpringBoot 工程是一个 jar 工程，没有 webapp，官方定义了静态资源的默认查找路径，我们也可以自定义该路径

默认的静态资源路径：

- classpath:/META-INF/resources/
- classpath:/resources/
- classpath:/static/
- classpath:/public/

classpath 为 resources 文件夹



自定义静态资源路径

```yaml
spring:
  web:
    resources:
      # 配置静态资源地址,如果设置,会覆盖默认值
      static-locations: classpath:/webapp
```

通过以上设置，静态资源的位置为 resources/webapp



## 4.4 拦截器（SpringMVC 配置）

第一步：interceptor 包下声明拦截器

```java
@Component
public class MyInterceptor implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("MyInterceptor拦截器的preHandle方法执行....");
        return true;
    }
}

```

第二步：拦截器配置

正常使用配置类，保证配置类在启动类的同包或子包即可生效

```java
@Configuration
public class SpringMvcConfig implements WebMvcConfigurer {
    @Resource
    private MyInterceptor myInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        registry.addInterceptor(myInterceptor);
    }
}
```







# 五、SpringBoot3 整合 Druid 数据源

第一步：创建程序

第二步：引入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.5</version>
    </parent>

    <groupId>com.shameyanng</groupId>
    <artifactId>springboot-003-druid</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <!--  web开发的场景启动器 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!-- 数据库相关配置启动器 jdbctemplate 事务相关 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <!-- druid启动器的依赖 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-3-starter</artifactId>
            <version>1.2.20</version>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <version>8.0.33</version>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>1.18.30</version>
        </dependency>
    </dependencies>
</project>
```

第三步：编写启动类

```java
@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }
}
```

第四步：编写配置文件

> 添加 druid 连接池的基本配置

```yaml
spring:
  datasource:
    # 连接池类型 
    type: com.alibaba.druid.pool.DruidDataSource

    # Druid的其他属性配置 springboot3整合情况下,数据库连接信息必须在Druid属性下!
    druid:
      url: jdbc:mysql://localhost:3306/xxx
      username: root
      password: xxx
      driver-class-name: com.mysql.cj.jdbc.Driver
      # 初始化时建立物理连接的个数
      initial-size: 5
      # 连接池的最小空闲数量
      min-idle: 5
      # 连接池最大连接数量
      max-active: 20
      # 获取连接时最大等待时间，单位毫秒
      max-wait: 60000
      # 申请连接的时候检测，如果空闲时间大于timeBetweenEvictionRunsMillis，执行validationQuery检测连接是否有效。
      test-while-idle: true
      # 既作为检测的间隔时间又作为testWhileIdel执行的依据
      time-between-eviction-runs-millis: 60000
      # 销毁线程时检测当前连接的最后活动时间和当前时间差大于该值时，关闭当前连接(配置连接在池中的最小生存时间)
      min-evictable-idle-time-millis: 30000
      # 用来检测数据库连接是否有效的sql 必须是一个查询语句(oracle中为 select 1 from dual)
      validation-query: select 1
      # 申请连接时会执行validationQuery检测连接是否有效,开启会降低性能,默认为true
      test-on-borrow: false
      # 归还连接时会执行validationQuery检测连接是否有效,开启会降低性能,默认为true
      test-on-return: false
      # 是否缓存preparedStatement, 也就是PSCache,PSCache对支持游标的数据库性能提升巨大，比如说oracle,在mysql下建议关闭。
      pool-prepared-statements: false
      # 要启用PSCache，必须配置大于0，当大于0时，poolPreparedStatements自动触发修改为true。在Druid中，不会存在Oracle下PSCache占用内存过多的问题，可以把这个数值配置大一些，比如说100
      max-pool-prepared-statement-per-connection-size: -1
      # 合并多个DruidDataSource的监控数据
      use-global-data-source-stat: true

logging:
  level:
    root: debug
```

第五步：编写 Controller

```java
@Slf4j
@RestController
@RequestMapping("stu")
public class StuController {
    @Resource
    private JdbcTemplate jdbcTemplate;

    @GetMapping("getStu")
    public Student getStu() {
        String sql = "select * from t_student where sno = ?";
        Student student = jdbcTemplate.queryForObject(sql, new BeanPropertyRowMapper<>(Student.class), 2);
        log.info("查询的 student 数据为:{}", student.toString());
        return student;
    }
}
```

第六步：启动测试









# 六、SpringBoot3 整合 Mybatis

## 6.1 实现过程

第一步：创建工程

第二步：引入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.1.5</version>
    </parent>

    <groupId>com.shameyanng</groupId>
    <artifactId>springboot-004-mybatis</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>3.0.2</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-3-starter</artifactId>
            <version>1.2.20</version>
        </dependency>
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
    <dependencyManagement>
        <dependencies>
            <!--解决spring-boot-starter-web中依赖的漏洞-->
            <dependency>
                <groupId>org.yaml</groupId>
                <artifactId>snakeyaml</artifactId>
                <version>2.0</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
</project>
```

第三步：编写配置文件

```yaml
server:
  port: 80
  servlet:
    context-path: /
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    druid:
      url: jdbc:mysql://localhost:3306/forssm
      username: root
      password: 123456
      driver-class-name: com.mysql.cj.jdbc.Driver

mybatis:
  configuration: # settings
    auto-mapping-behavior: full
    map-underscore-to-camel-case: true
    log-impl: org.apache.ibatis.logging.slf4j.Slf4jImpl
  type-aliases-package: com.shameyang.pojo # 别名
  mapper-locations: classpath:/mappers/*.xml # mapper文件位置
```

第四步：编写实体类

```java
@Data
public class Student {
    private String sno;
    private String sname;
}
```

第五步：Mapper 接口和实现（XML）

```java
public interface StuMapper {
    List<Student> queryAll();
}
```

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "https://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.shameyang.mapper.StuMapper">
    <select id="queryAll" resultType="student">
        select * from t_student
    </select>
</mapper>
```

第六步：编写 service 层

```java
public interface StuService {
    List<Student> stuList();
}
```

```java
@Service
public class StuServiceImpl implements StuService {
    @Resource
    private StuMapper stuMapper;

    @Override
    public List<Student> stuList() {
        return stuMapper.queryAll();
    }
}
```

第七步：编写 controller 层

```java
@RestController
@RequestMapping("student")
public class StuController {
    @Resource
    private StuServiceImpl stuService;

    @GetMapping("list")
    public List<Student> getStudent() {
        return stuService.stuList();
    }
}
```

第八步：编写启动类和 mapper 接口扫描

```java
@MapperScan("com.shameyang.mapper")
@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }
}
```

第九步：启动测试



## 6.2 声明式事务整合

引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

注：SpringBoot项目会自动配置一个 DataSourceTransactionManager，所以我们只需在方法（或者类）加上 @Transactional 注解，就自动纳入 Spring 的事务管理了

```java
@Transactional
public void update(){
    User user = new User();
    user.setId(1);
    user.setPassword("test2");
    user.setAccount("test2");
    userMapper.update(user);
}
```



## 6.2 AOP 整合

引入依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-aop</artifactId>
</dependency>
```

直接使用aop注解即可：

```java
@Component
@Aspect
public class LogAdvice {
    @Before("execution(* com..service.*.*(..))")
    public void before(JoinPoint joinPoint){
        System.out.println("LogAdvice.before");
        System.out.println("joinPoint = " + joinPoint);
    }

}
```







# 七、SpringBoot3 项目打包和运行

## 7.1 添加打包插件

> 在 Spring Boot 项目中添加 `spring-boot-maven-plugin` 插件是为了支持将项目打包成可执行的可运行 jar 包。如果不添加 `spring-boot-maven-plugin` 插件配置，使用常规的 java -jar 命令来运行打包后的 Spring Boot 项目是无法找到应用程序的入口点，因此导致无法运行

```xml
<!-- SpringBoot应用打包插件 -->
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```



## 7.2 执行打包

在 idea 点击 package 进行打包

可以在编译的 target 文件中查看 jar 包



## 7.3 命令启动和参数说明

java -jar 命令用于在 Java 环境中执行可执行的 JAR 文件

```
命令格式：java -jar [选项] [参数] <jar文件名>
```

参数说明：

- `-D<name>=<value>`：设置系统属性，例如：`java -jar -Dserver.port=8080 myapp.jar`。可以通过 System.getProperty() 方法在应用程序中获取该属性值

- `-Dspring.profiles.active=<profile>`：指定 Spring Boot 的激活配置文件，可以通过 application-.properties 或 application-.yml 文件来加载相应的配置。例如：`java -jar -Dspring.profiles.active=dev myapp.jar`

- `-X`：设置 JVM 参数，例如内存大小、垃圾回收策略等

  常用的选项包括：

  - `-Xmx<size>`：设置 JVM 的最大堆内存大小，例如 `-Xmx512m` 表示设置最大堆内存为512MB
  - `-Xms<size>`：设置 JVM 的初始堆内存大小，例如 `-Xms256m` 表示设置初始堆内存为256MB
