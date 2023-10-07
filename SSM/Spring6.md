# 一、初识 Spring

## 1.1 OCP 开闭原则

Open Close Principle，软件七大开发原则中最基本的原则，其他六个原则都为这个原则服务

- Open：对扩展开放
- Close：对修改关闭

在进行系统功能扩展后，不修改之前的程序，这就是 OCP 原则



## 1.2 DIP 依赖倒置原则

Dependence Inversion Principle，软件七大开发原则之一

- Dependence：上层与下层的依赖关系
- Inversion：让上层与下层不再依赖

DIP 原则，主要倡导面向接口编程，面向抽象编程，使上下层之间不再依赖，从而降低程序的耦合度，提高扩展力



## 1.3 IoC 控制反转思想

Inversion of Control，面向对象编程的一种思想（或者叫一种新型设计模式，出现较晚，没有被纳入 GoF 23种设计模式中），可以降低代码之间的耦合度，符合 DIP 原则

控制反转原则的核心：将对象的创建权、对象和对象之间关系的管理权交出去，由第三方容器负责创建和维护



## 1.4 依赖注入

控制反转常见的实现方式：依赖注入（Dependency Injection，简称 DI）

- 依赖：对象和对象的关系
- 注入：一种手段，可以让对象与对象间产生关系

- 通常，依赖注入的实现包括两种方式：
  - set 方法注入
  - 构造方法注入



## 1.5 初识 Spring

Spring 框架是一个实现了 IoC 思想的容器

- 可以帮我们 new 对象
- 可以帮我们维护对象和对象之间的关系







# 二、Spring 概述

## 2.1 Spring 简介

Spring 是一个

- 开源框架，由 Rod JohnSon 创建。它是为了解决企业应用开发的复杂性而创建的

- 轻量级的控制反转（IoC）和面向切面（AOP）的容器框架

最初的出现是为了解决 EJB （Enterprise JavaBean）臃肿的设计，以及难以测试等问题



## 2.2 Spring 八大模块

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/%E5%85%AB%E5%A4%A7%E6%A8%A1%E5%9D%97.png" style="zoom: 80%;float:left" />



Spring Core 模块

- 这是 Spring 框架最基础的部分，它提供了依赖注入（Dependency Injection）特征来实现容器对 Bean 的管理。核心容器的主要组件是 BeanFactory，BeanFactory 是工厂模式的一个实现，是任何 Spring 应用的核心。它使用 IoC 将应用配置和依赖从实际的应用代码中分离出来

Spring Context 模块

- 如果说核心模块中的 BeanFactory 使 Spring 成为容器的话，那么上下文模块就是 Spring 成为框架的原因

- 这个模块扩展了 BeanFactory，增加了对国际化（I18N）消息、事件传播、验证的支持。另外提供了许多企业服务，例如电子邮件、JNDI 访问、EJB 集成、远程以及时序调度（scheduling）服务。也包括了对模版框架例如 Velocity 和 FreeMarker 集成的支持

Spring AOP 模块

- Spring 在它的 AOP 模块中提供了对面向切面编程的丰富支持，Spring AOP 模块为基于 Spring 的应用程序中的对象提供了事务管理服务。通过使用 Spring AOP，不用依赖组件，就可以将声明性事务管理集成到应用程序中，可以自定义拦截器、切点、日志等操作

Spring DAO 模块

- 提供了一个 JDBC 的抽象层和异常层次结构，消除了繁琐的 JDBC 编码和数据库厂商特有的错误代码解析，用于简化JDBC

Spring ORM 模块

- Spring 提供了 ORM 模块。Spring 并不试图实现它自己的 ORM 解决方案，而是为几种流行的 ORM 框架提供了集成方案，包括 Hibernate、JDO 和 iBATIS SQL 映射，这些都遵从 Spring 的通用事务和 DAO 异常层次结构

Spring Web MVC 模块

- Spring 为构建 Web 应用提供了一个功能全面的 MVC 框架。虽然 Spring 可以很容易地与其它 MVC 框架集成，例如 Struts，但 Spring 的 MVC 框架使用 IoC 对控制逻辑和业务对象提供了完全的分离

Spring WebFlux 模块

- Spring Framework 中包含的原始 Web 框架 Spring Web MVC 是专门为 Servlet API 和 Servlet 容器构建的。反应式堆栈 Web 框架 Spring WebFlux 是在 5.0 版的后期添加的。它是完全非阻塞的，支持反应式流(Reactive Stream)背压，并在 Netty，Undertow 和 Servlet 3.1+容器等服务器上运行

Spring Web 模块

- Web 上下文模块建立在应用程序上下文模块之上，为基于 Web 的应用程序提供了上下文，提供了 Spring 和其它 Web 框架的集成，比如 Struts、WebWork。还提供了一些面向服务支持，例如：实现文件上传的 multipart 请求



## 2.3 Spring 特点

轻量

- 从大小与开销两方面而言 Spring 都是轻量的。完整的 Spring 框架可以在一个大小只有 1MB 多的 JAR 文件里发布。并且 Spring 所需的处理开销也是微不足道的
- Spring 是非侵入式的：Spring 应用中的对象不依赖于 Spring 的特定类

控制反转

- Spring 通过一种称作控制反转（IoC）的技术促进了松耦合。当应用了 IoC，一个对象依赖的其它对象会通过被动的方式传递进来，而不是这个对象自己创建或者查找依赖对象。你可以认为 IoC 与 JNDI 相反——不是对象从容器中查找依赖，而是容器在对象初始化时不等对象请求就主动将依赖传递给它

面向切面

- Spring 提供了面向切面编程的丰富支持，允许通过分离应用的业务逻辑与系统级服务（例如审计【auditing】和事务【transaction】管理）进行内聚性的开发。应用对象只实现它们应该做的——完成业务逻辑——仅此而已。它们并不负责（甚至是意识）其它的系统级关注点，例如日志或事务支持

容器

- Spring 包含并管理应用对象的配置和生命周期，在这个意义上它是一种容器，你可以配置你的每个 bean 如何被创建——基于一个可配置原型（prototype），你的 bean 可以创建一个单独的实例或者每次需要时都生成一个新的实例——以及它们是如何相互关联的。然而，Spring 不应该被混同于传统的重量级的 EJB 容器，它们经常是庞大与笨重的，难以使用

框架

- Spring 可以将简单的组件配置、组合成为复杂的应用。在 Spring 中，应用对象被声明式地组合，典型地是在一个 XML 文件里。Spring 也提供了很多基础功能（事务管理、持久化框架集成等等），将应用逻辑的开发留给了你

所有 Spring 的这些特征使你能够编写更干净、更可管理、并且更易于测试的代码。它们也为 Spring 中的各种模块提供了基础支持







# 三、Spring 入门程序

## 3.1 下载 Spring

第一步：进入 [官网](https://spring.io/) 或 [中文官网](http://spring.p2hp.com/)，Projects --> Spring Framework

第二步：点击 GitHub 图标

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/spring%E4%B8%8B%E8%BD%BD1.png)

第三步：找到下图位置，点击超链接

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/spring%E4%B8%8B%E8%BD%BD2.png)

第四步：找到下图位置，点击超链接

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/spring%E4%B8%8B%E8%BD%BD3.png)

第五步：选择对应的版本，这里以 5.3.9 为例

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/spring%E4%B8%8B%E8%BD%BD4.png)

第六步：下载压缩包并解压

目录：

- docs：spring 框架的 API 帮助文档
- libs：spring 框架的 jar 文件
- schemas：spring 框架的 XML 配置文件相关的约束文件



## 3.2 Spring 的 jar 包

| **JAR文件**                      | **描述**                                                     |
| :------------------------------- | :----------------------------------------------------------- |
| spring-aop-5.3.9.jar             | **这个 jar 文件包含在应用中使用 Spring 的 AOP 特性时所需的类** |
| spring-aspects-5.3.9.jar         | **提供对 AspectJ 的支持，以便可以方便的将面向切面的功能集成进 IDE 中** |
| spring-beans-5.3.9.jar           | **这个 jar 文件是所有应用都要用到的，它包含访问配置文件、创建和管理 bean 以及进行 Inversion ofControl / Dependency Injection（IoC/DI）操作相关的所有类。如果应用只需基本的 IoC/DI 支持，引入 spring-core.jar  及 spring-beans.jar 文件就可以了。** |
| spring-context-5.3.9.jar         | **这个 jar 文件为 Spring 核心提供了大量扩展。可以找到使用 Spring ApplicationContext 特性时所需的全部类，JDNI 所需的全部类，instrumentation 组件以及校验 Validation 方面的相关类。** |
| spring-context-indexer-5.3.9.jar | 虽然类路径扫描非常快，但是 Spring 内部存在大量的类，添加此依赖，可以通过在编译时创建候选对象的静态列表来提高大型应用程序的启动性能。 |
| spring-context-support-5.3.9.jar | 用来提供 Spring 上下文的一些扩展模块,例如实现邮件服务、视图解析、缓存、定时任务调度等 |
| spring-core-5.3.9.jar            | **Spring 框架基本的核心工具类。Spring 其它组件要都要使用到这个包里的类，是其它组件的基本核心，当然你也可以在自己的应用系统中使用这些工具类。** |
| spring-expression-5.3.9.jar      | Spring 表达式语言。                                          |
| spring-instrument-5.3.9.jar      | Spring3.0对服务器的代理接口。                                |
| spring-jcl-5.3.9.jar             | Spring 的日志模块。JCL，全称为"Jakarta Commons Logging"，也可称为"Apache Commons Logging"。 |
| spring-jdbc-5.3.9.jar            | **Spring对 JDBC 的支持。**                                   |
| spring-jms-5.3.9.jar             | 这个 jar 包提供了对 JMS 1.0.2/1.1的支持类。JMS 是 Java 消息服务。属于 JavaEE 规范之一。 |
| spring-messaging-5.3.9.jar       | 为集成 messaging api 和消息协议提供支持                      |
| spring-orm-5.3.9.jar             | **Spring 集成 ORM 框架的支持，比如集成 hibernate，mybatis 等。** |
| spring-oxm-5.3.9.jar             | 为主流 O/X Mapping 组件提供了统一层抽象和封装，OXM 是 Object Xml Mapping。对象和 XML 之间的相互转换。 |
| spring-r2dbc-5.3.9.jar           | Reactive Relational Database Connectivity (关系型数据库的响应式连接) 的缩写。这个 jar 文件是 Spring 对 r2dbc 的支持。 |
| spring-test-5.3.9.jar            | 对 Junit 等测试框架的简单封装。                              |
| spring-tx-5.3.9.jar              | **为 JDBC、Hibernate、JDO、JPA、Beans 等提供的一致的声明式和编程式事务管理支持。** |
| spring-web-5.3.9.jar             | **Spring 集成 MVC 框架的支持，比如集成 Struts 等。**         |
| spring-webflux-5.3.9.jar         | **WebFlux 是 Spring5 添加的新模块，用于 web 的开发，功能和 SpringMVC 类似的，Webflux 使用当前一种比较流程响应式编程出现的框架。** |
| spring-webmvc-5.3.9.jar          | **SpringMVC 框架的类库**                                     |
| spring-websocket-5.3.9.jar       | Spring 集成 WebSocket 框架时使用                             |



## 3.3 第一个 Spring 程序

第一步：先新建一个 Maven 模块，然后在 pom.xml 文件中添加 spring context 依赖和 junit 依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.shameyanng</groupId>
    <artifactId>spring6-001-first</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <repositories>
        <repository>
            <id>spring-milestones</id>
            <name>Spring Milestones</name>
            <url>https://repo.spring.io/milestone</url>
            <snapshots>
                <enabled>false</enabled>
            </snapshots>
        </repository>
    </repositories>

    <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>6.1.0-M2</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13.2</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>
</project>
```

引入 spring context 依赖后，会关联引入其他依赖：

- spring aop：面向切面编程
- spring beans：IoC 核心
- spring core：spring 的核心工具包
- spring jcl：spring 的日志包
- spring expression：spring 表达式

<img src="https://cdn.jsdelivr.net/gh/ShameYang/images/img/spring-%E5%85%B3%E8%81%94%E5%BC%95%E5%85%A5%E7%9A%84%E4%BE%9D%E8%B5%96.png" style="zoom: 80%;float:left" />

第二步：定义 bean，这里以 User 为例

```java
public class User {
}
```

第三步：类的根路径下（src/main/java/resource），新建 spring 的配置文件：beans.xml，并配置 bean

IDEA 中自带 spring 配置文件的模板，New --> XML Configuration File --> Spring Config

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userBean" class="com.shameyang.spring6.bean.User"/>
</beans>
```

第四步：编写测试程序

```java
public class Spring6Test {
    @Test
    public void testFirst() {
        // 初始化 Spring 容器上下文（解析 beans.xml 文件，创建所有的 bean 对象）
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml");
        // 根据 id 获取 bean 对象
        Object userBean = applicationContext.getBean("userBean");
        System.out.println(userBean);
    }
}
```

第五步：运行



## 3.4 第一个 Spring 程序分析

1. ```xml
   <bean id="userBean" class="com.shameyang.spring6.bean.User"/>
   ```

   id 不能重复，对象的唯一标识

   class 用来指定要创建的 Java 对象的类名，必须是全类名

2. bean 可以是任意类，只要该类不是抽象的，并且提供了无参构造方法，例如 Date

   ```xml
   <bean id="dateBean" class="java.util.Date"/>
   ```

3. spring 通过调用类的无参构造方法来创建对象，所以如果想要 spring 为我们创建对象，必须保证无参构造方法存在

   原理如下：

   ```java
   Class clazz = Class.forName("com.shameyang.spring6.bean.User");
   Object obj = clazz.newInstance();
   ```

3. spring 配置文件可以有多个，名字由我们负责提供

   ```java
   ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml", "xxx.xml", ...);
   ```

4. 创建好的对象存储的数据结构：Map<String, Object>

   ```java
   // 根据 id 获取 bean 对象
   Object userBean = applicationContext.getBean("userBean");
   ```

5. getBean() 方法返回类型是 Object，如果访问子类特有的属性或方法，需要向下转型

   ```java
   User user = applicationContext.getBean("userBean", User.class);
   ```

6. ClassPathXmlApplicationContext 是从类路径中加载配置文件，如果没有在类路径当中，需要使 FileSystemXmlApplicationContext 类进行加载配置文件

   ```java
   ApplicationContext applicationContext2 = new FileSystemXmlApplicationContext("spring.xml's path");
   BeanClass beanClass = applicationContext2.getBean("beanId", BeanClass.class);
   System.out.println(beanClass);
   ```



## 3.5 Spring6启用 Log4j2日志框架

Spring5之后，Spring 框架支持 Log4j2。下面演示如何启用该日志框架：

第一步：引入依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-core</artifactId>
        <version>2.20.0</version>
    </dependency>
    <dependency>
        <groupId>org.apache.logging.log4j</groupId>
        <artifactId>log4j-slf4j2-impl</artifactId>
        <version>2.20.0</version>
    </dependency>
</dependencies>
```

第二步：类的根路径下提供 log4j2.xml 配置文件（名字固定，必须放在根路径下）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
    <loggers>
        <!--
            level指定日志级别，从低到高的优先级：
                ALL < TRACE < DEBUG < INFO < WARN < ERROR < FATAL < OFF
        -->
        <root level="DEBUG">
            <appender-ref ref="spring6log"/>
        </root>
    </loggers>

    <appenders>
        <!--输出日志信息到控制台-->
        <console name="spring6log" target="SYSTEM_OUT">
            <!--控制日志输出的格式-->
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss SSS} [%t] %-3level %logger{1024} - %msg%n"/>
        </console>
    </appenders>
</configuration>
```



使用框架

```java
Logger logger = LoggerFactory.getLogger(Clazz.class);
logger.info(logger);
```







# 四、Spring 对 IoC 的实现

## 4.1 set 注入

通过 set 方法给属性赋值

```java
public class UserDao {
    public void insert() {
        System.out.println("UserDao insert...");
    }
}
```

```java
public class UserService {
    private UserDao userDao;

    // 使用 set 方式注入，必须提供 set 方法
    // 反射机制要调用这个方法给属性赋值
    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save(){
        userDao.insert();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userDaoBean" class="com.shameyang.spring6.dao.UserDao"/>

    <bean id="userServiceBean" class="com.shameyang.spring6.service.UserService">
        <property name="userDao" ref="userDaoBean"/>
    </bean>
</beans>
```

```java
public class DITest {
    @Test
    public void testDI() {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        UserService userService = applicationContext.getBean("userServiceBean", UserService.class);
        userService.save();
    }
}
```



## 4.2 构造注入

通过构造方法给属性赋值

```java
public class OrderDao {
    public void deleteById() {
        System.out.println("OrderDao delete...");
    }
}
```

```java
public class OrderService {
    private OrderDao orderDao;

    // 通过反射机制调用构造方法给属性赋值
    public OrderService(OrderDao orderDao) {
        this.orderDao = orderDao;
    }

    public void delete() {
        orderDao.deleteById();
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <bean id="orderDaoBean" class="com.shameyang.spring6.dao.OrderDao"/>
    <bean id="orderServiceBean" class="com.shameyang.spring6.service.OrderService">
        <!-- 通过下标注入 -->
        <constructor-arg index="0" ref="orderDaoBean"/>
        <!-- 通过参数名注入 -->
        <constructor-arg name="orderDao" ref="orderDaoBean">
        <!-- 自动推断类型 -->
        <constructor-arg ref="orderDaoBean">
    </bean>
</beans>
```

```java
public class DITest {
    @Test
    public void testConstructorDI() {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        OrderService orderService = applicationContext.getBean("orderServiceBean", OrderService.class);
        orderService.delete();
    }
}
```



## 4.3 set 注入专题

### 注入外部 Bean（常用）：使用 ref 属性

```xml
<bean id="userDaoBean" class="com.shameyang.spring6.dao.UserDao"/>

<bean id="userServiceBean" class="com.shameyang.spring6.service.UserService">
	<property name="userDao" ref="userDaoBean"/>
</bean>
```

### 注入内部 Bean（了解）：嵌套 bean 标签

```xml
<bean id="userServiceBean" class="com.shameyang.spring6.service.UserService">
	<property name="userDao">
	<bean class="com.shameyang.spring6.dao.UserDao"/>
	</property>
</bean>
```

### 注入简单类型：使用 value 属性或 value 标签

```xml
<bean id="userBean" class="com.shameyang.spring6.bean.User">
	<!-- value 属性 -->
	<property name="age" value="20"/>
	<!-- value 标签 -->
	<property name="age">
    	<value>20</value>
	</property>
</bean>
```

简单类型有哪些？

- 基本数据类型、以及对应的包装类
- Enum 子类
- String 或其它的 CharSequence 子类
- Number 子类
- Date 子类
- Temporal 子类
- URI、URL
- Locale
- Class

Spring 源码如下

```java
public class BeanUtils{
    
    //.......
    
    /**
	 * Check if the given type represents a "simple" property: a simple value
	 * type or an array of simple value types.
	 * <p>See {@link #isSimpleValueType(Class)} for the definition of <em>simple
	 * value type</em>.
	 * <p>Used to determine properties to check for a "simple" dependency-check.
	 * @param type the type to check
	 * @return whether the given type represents a "simple" property
	 * @see org.springframework.beans.factory.support.RootBeanDefinition#DEPENDENCY_CHECK_SIMPLE
	 * @see org.springframework.beans.factory.support.AbstractAutowireCapableBeanFactory#checkDependencies
	 * @see #isSimpleValueType(Class)
	 */
	public static boolean isSimpleProperty(Class<?> type) {
		Assert.notNull(type, "'type' must not be null");
		return isSimpleValueType(type) || (type.isArray() && isSimpleValueType(type.getComponentType()));
	}

	/**
	 * Check if the given type represents a "simple" value type: a primitive or
	 * primitive wrapper, an enum, a String or other CharSequence, a Number, a
	 * Date, a Temporal, a URI, a URL, a Locale, or a Class.
	 * <p>{@code Void} and {@code void} are not considered simple value types.
	 * @param type the type to check
	 * @return whether the given type represents a "simple" value type
	 * @see #isSimpleProperty(Class)
	 */
	public static boolean isSimpleValueType(Class<?> type) {
		return (Void.class != type && void.class != type &&
				(ClassUtils.isPrimitiveOrWrapper(type) ||
				Enum.class.isAssignableFrom(type) ||
				CharSequence.class.isAssignableFrom(type) ||
				Number.class.isAssignableFrom(type) ||
				Date.class.isAssignableFrom(type) ||
				Temporal.class.isAssignableFrom(type) ||
				URI.class == type ||
				URL.class == type ||
				Locale.class == type ||
				Class.class == type));
	}
    
    //........
}
```

注意：Date 当作简单类型时，注意日期格式

### 级联属性赋值（了解）

要点：

1. spring 配置文件要注意顺序，先 ref 再 value
2. 级联属性必须有 getter 方法

```xml
<bean id="clazzBean" class="com.shameyang.spring6.beans.Clazz"/>

    <bean id="student" class="com.shameyang.spring6.beans.Student">
        <property name="name" value="张三"/>
        <!-- 要点1：以下两行配置的顺序不能颠倒 -->
        <property name="clazz" ref="clazzBean"/>
        <!-- 要点2：clazz 属性必须有 getter 方法 -->
        <property name="clazz.name" value="高三一班"/>
    </bean>
</beans>
```

### 注入数组

简单类型用 value

```xml
<bean id="person" class="com.shameyang.spring6.bean.Person">
    <property name="hobbies">
        <array>
            <value>sing</value>
            <value>jump</value>
            <value>rap</value>
        </array>
    </property>
</bean>
```

非简单类型用 ref

```xml
<bean id="goods1" class="com.shameyang.spring6.beans.Goods">
    <property name="name" value="西瓜"/>
</bean>

<bean id="goods2" class="com.shameyang.spring6.beans.Goods">
    <property name="name" value="苹果"/>
</bean>

<bean id="order" class="com.shameyang.spring6.beans.Order">
    <property name="goods">
        <array>
            <ref bean="goods1"/>
            <ref bean="goods2"/>
        </array>
    </property>
</bean>
```

### 注入 List、Set 集合

将上边注入数组的 array 标签改为集合对应的标签即可，集合中元素是简单类型用 value，非简单类型用 ref

