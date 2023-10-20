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

### 注入内部 Bean（了解）

嵌套 bean 标签

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
<bean id="clazzBean" class="com.shameyang.spring6.bean.Clazz"/>

    <bean id="student" class="com.shameyang.spring6.bean.Student">
        <property name="name" value="张三"/>
        <!-- 要点1：以下两行配置的顺序不能颠倒 -->
        <property name="clazz" ref="clazzBean"/>
        <!-- 要点2：clazz 属性必须有 getter 方法 -->
        <property name="clazz.name" value="高三一班"/>
    </bean>
</beans>
```

### 注入数组

简单类型 array 嵌套 value

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

非简单类型 array 嵌套 ref

```xml
<bean id="goods1" class="com.shameyang.spring6.bean.Goods">
    <property name="name" value="西瓜"/>
</bean>

<bean id="goods2" class="com.shameyang.spring6.bean.Goods">
    <property name="name" value="苹果"/>
</bean>

<bean id="order" class="com.shameyang.spring6.bean.Order">
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

### 注入 Map 集合

map 标签嵌套 entry 标签

```xml
<bean id="peopleBean" class="com.shameyang.spring6.bean.People">
	<property name="addrs">
		<map>
			<!-- 非简单类型，使用 key-ref 或 value-ref 属性 -->
			<entry key="1" value="北京"/>
			<entry key="2" value="上海"/>
			<entry key="3" value="深圳"/>
		</map>
	</property>
</bean>
```

### 注入 Properties

props 标签嵌套 prop 标签

```xml
<bean id="peopleBean" class="com.shameyang.spring6.bean.People">
	<property name="properties">
		<props>
			<prop key="driver">com.mysql.cj.jdbc.Driver</prop>
			<prop key="url">jdbc:mysql://localhost:3306/spring</prop>
			<prop key="username">root</prop>
			<prop key="password">123456</prop>
		</props>
	</property>
</bean>
```

### 注入 null 和空字符串

注入null：

- 不给属性赋值
- \<null/>

注入空字符串：

- value = ""
- \<value/>

### 注入的值中含有特殊符号

方案一：转义字符

| 特殊字符 | 转义字符 |
| -------- | -------- |
| >        | \&gt;    |
| <        | \&lt;    |
| '        | \&apos;  |
| "        | \&quot;  |
| &        | \&amp;   |

方案二：\<![CDATA[放在这里]]>，放在 CDATA 区中的数据不会被 XML 文件解析器解析

```xml
<bean id="mathBean" class="com.shameyang.spring6.bean.Math">
	<property name="result">
		<!-- 只能使用 value 标签 -->
		<value><![CDATA[2 < 3]]></value>
	</property>
</bean>
```



## 4.4 p 命名空间注入

基于 p 命名空间注入，可以简化之前的 set 方法注入

第一步：在 spring 配置文件头部添加：xmlns:p="http://www.springframework.org/schema/p"

第二步：对应的属性提供 setter 方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="customerBean" class="com.shameyang.spring6.bean.Customer" p:name="zhangsan" p:age="20"/>

</beans>
```



## 4.5 c 命名空间注入

基于 c 命名空间注入，可以简化之前的构造方法注入

第一步：spring 配置文件头部添加：xmlns:c="http://www.springframework.org/schema/c"

第二步：提供构造方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:c="http://www.springframework.org/schema/c"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--<bean id="myTimeBean" class="com.shameyang.spring6.bean.MyTime" c:year="2000" c:month="1" c:day="1"/>-->

    <bean id="myTimeBean" class="com.shameyang.spring6.bean.MyTime" c:_0="2000" c:_1="1" c:_2="1"/>

</beans>
```



## 4.6 util 命名空间

使用 util 命名空间可以实现配置复用

引入 util 命名空间：在 spring 配置文件头部添加如下配置信息

![](https://cdn.jsdelivr.net/gh/ShameYang/images/img/spring-%E5%BC%95%E5%85%A5util%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4.png)

例如：不同数据源共用数据库信息

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

    <util:properties id="prop">
        <prop key="driver">com.mysql.cj.jdbc.Driver</prop>
        <prop key="url">jdbc:mysql://localhost:3306/spring</prop>
        <prop key="username">root</prop>
        <prop key="password">123456</prop>
    </util:properties>

    <bean id="dataSource1" class="com.shameyang.spring6.bean.MyDataSource1">
        <property name="properties" ref="prop"/>
    </bean>

    <bean id="dataSource2" class="com.shameyang.spring6.bean.MyDataSource2">
        <property name="properties" ref="prop"/>
    </bean>
</beans>
```



## 4.7 基于 XML 的自动装配

Spring 可以完成自动化注入，自动化注入又称为自动装配

### 根据名称自动装配

```xml
<bean id="userService" class="com.shameyang.spring6.service.UserService" autowire="byName"/>
<!-- id 对应 set 方法的属性名 -->
<bean id="userDao" class="com.shameyang.spring6.dao.UserDao"/>
```

### 根据类型自动装配

```xml
<!--byType表示根据类型自动装配-->
<bean id="userService" class="com.shameyang.spring6.service.UserService" autowire="byType"/>

<bean class="com.shameyang.spring6.dao.UserDao"/>
```



## 4.8 引入外部属性配置文件

编写数据源时需要连接数据库的信息，我们可以将信息写到外部配置文件中，这样用户修改会非常方便。下面介绍如何在 spring 中引入外部属性配置文件

第一步：定义一个数据源类，并提供相关属性

```java
public class MyDataSource implements DataSource {
    @Override
    public String toString() {
        return "MyDataSource{" +
                "driver='" + driver + '\'' +
                ", url='" + url + '\'' +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }

    private String driver;
    private String url;
    private String username;
    private String password;

    public void setDriver(String driver) {
        this.driver = driver;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    //......
}
```

第二步：类路径下新建 jdbc.properties 文件，配置信息

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring
jdbc.username=root
jdbc.password=root123
```

第三步：在 spring 配置文件中引入 context 命名空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

</beans>
```

第四步：spring 中配置使用 jdbc.properties 文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

    <context:property-placeholder location="jdbc.properties"/>
    
    <bean id="dataSource" class="com.shameyang.spring6.bean.MyDataSource">
        <property name="driver" value="${jdbc.driver}"/>
        <property name="url" value="${jdbc.url}"/>
        <property name="username" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
</beans>
```







# 五、Bean 的作用域

我们可以在 bean 标签中，使用 scope 属性配置 Bean 的作用域

scope 的值有如下几种：

- singleton（default）：默认的，单例
- prototype：原型。每调用一次 getBean() 方法则获取一个新的 Bean 对象，或每次注入的时候都是新对象
- request：一个请求对应一个 Bean（**仅限于在 WEB 应用中使用**）
- session：一个会话对应一个 Bean（**仅限于在 WEB 应用中使用**）
- global session：**portlet 应用中专用的**。如果在 Servlet 的 WEB 应用中使用 global session 的话，和 session 一个效果。（portlet 和 servlet 都是规范。servlet 运行在servlet 容器中，例如Tomcat。portlet 运行在 portlet 容器中）
- application：一个应用对应一个 Bean（**仅限于在 WEB 应用中使用**）
- websocket：一个 websocket 生命周期对应一个 Bean（**仅限于在 WEB 应用中使用**）
- 自定义scope：很少使用



自定义一个 Scope（了解即可）：线程级别的 Scope

第一步：自定义 Scope（实现 Scope 接口）

- spring 中内置了线程范围的类：org.springframework.context.support.SimpleThreadScope，可以直接使用

第二步：将自定义 Scope 注册到 Spring 容器中

```xml
<bean class="org.springframework.beans.factory.config.CustomScopeConfigurer">
  <property name="scopes">
    <map>
      <entry key="myThread">
        <bean class="org.springframework.context.support.SimpleThreadScope"/>
      </entry>
    </map>
  </property>
</bean>
```

第三步：使用 Scope

```xml
<bean ... scope="myThread" />
```







# 六、Bean 的获取方式

Spring 提供了多种获取 Bean 对象的方式：

- 构造方法
- 简单工厂模式（静态工厂方法模式）
- factory-bean
- FactoryBean 接口



## 6.1 构造方法

我们之前使用的都是这种方式：在 spring 配置文件中直接配置类的全路径。默认情况下，会调用 Bean 的无参构造方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userBean" class="com.shameyang.spring6.bean.User"/>

</beans>
```



## 6.2 简单工厂模式（静态工厂方法模式）

假设已经定义了 UserBean。现在我们提供一个工厂类，工厂类里有一个静态方法

```java
public class UserFactory {
    public static User get() {
        return new User();
    }
}
```

然后在 spring 配置文件中指定创建该 Bean 的方法

```xml
<bean id="userBean" class="com.shameyang.spring6.bean.UserFactory" factory-method="get"/>
```



## 6.3 factory-bean（工厂方法模式）

假设已经定义了 UserBean。现在定义具体工厂类（一个 Bean 对应一个 BeanFactory），工厂类中提供实例方法

```java
public class UserFactory {
    public User get() {
        return new User();
    }
}
```

然后在 spring 配置文件中指定 factory-bean 和 factory-method

```xml
<bean id="userFactory" class="com.shameyang.spring6.bean.UserFactory"/>
<bean id="userBean" factory-bean="userFactory" factory-method="get"/>
```



## 6.4 实现 FactoryBean 接口

我们可以编写一个类实现 FactoryBean 接口，这样 factory-bean 和 factory-method 就无需手动配置了

- factory-bean 会自动指向实现 FactoryBean 接口的类
- factory-method 会自动指向 getObject() 方法

假设已经定义了 UserBean。现在编写一个类实现 FactoryBean 接口

```java
public class UserFactoryBean implements FactoryBean<User> {
    @Override
    public User getObject() throws Exception {
        return new User();
    }

    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        // true 表示单例
        // false 表示原型
        return true;
    }
}
```

在 spring 配置文件中配置 FactoryBean

```xml
<bean id="userBean" class="com.shameyang.spring6.bean.UserFactoryBean"/>
```







# 七、关于 FactoryBean

## 7.1 FactoryBean 和 BeanFactory 的区别、

FactoryBean 是一个 Bean，能够辅助 Spring 实例化其它 Bean 对象

BeanFactory 是一个工厂，是 Spring IoC 容器的顶级对象，负责创建 Bean 对象



## 7.2 注入自定义 Date

在之前的学习中，java.util.Date 在注入时，如果当作简单类型需要使用规定的格式：Mon Jan 01 00:00:00 CST 2023

如果想用自定义的格式（当作非简单类型），我们就可以使用 FactoryBean 来辅助完成

第一步：编写 DateFactoryBean 实现 FactoryBean 接口

```java
public class DateFactoryBean implements FactoryBean<Date> {
    private String date;
    
    public DateFactoryBean(String date) {
        this.date = date;
    }
    
    @Override
    public Date getObject() throws Exception {
        SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd");
        return sdf.parse(this.date);
    }
    
    @Override
    public Class<?> getObjectType() {
        return null;
    }
}
```

第二步：编写 spring 配置文件

```xml
<bean id="dateBean" class="com.shameyang.spring6.bean.DateFactoryBean">
	<constructor-arg name="date" value="2000-01-01"/>
</bean>

<bean id="..." class="...">
    <property name="..." ref="dateBean">
</bean>
```







# 八、Bean 的生命周期

## 8.1 Bean 的生命周期五步

第一步：实例化 Bean

第二步：Bean 属性赋值

第三步：初始化 Bean

第四步：使用 Bean

第五步：销毁 Bean



注意：

- 配置文件中的 init-method 指定初始化方法，destroy-method 指定销毁方法

- 只有 spring 正常关闭才会执行销毁方法

示例如下：

```java
public class User {
    private String name;

    public User() {
        System.out.println("1.实例化 Bean");
    }

    public void setName(String name) {
        this.name = name;
        System.out.println("2.Bean 属性赋值");
    }

    public void initBean(){
        System.out.println("3.初始化 Bean");
    }

    public void destroyBean(){
        System.out.println("5.销毁 Bean");
    }

}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--
    	init-method 属性指定初始化方法
    	destroy-method 属性指定销毁方法
    -->
    <bean id="userBean" class="com.shameyang.spring6.bean.User" init-method="initBean" destroy-method="destroyBean">
        <property name="name" value="zhangsan"/>
    </bean>

</beans>
```

```java
public class BeanLifecycleTest {
    @Test
    public void testLifecycle(){
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        User userBean = applicationContext.getBean("userBean", User.class);
        System.out.println("4.使用 Bean");
        // 只有正常关闭 spring 容器才会执行销毁方法
        ClassPathXmlApplicationContext context = (ClassPathXmlApplicationContext) applicationContext;
        context.close();
    }
}
```



## 8.2 Bean 的生命周期七步

在上面的五步中，如果想在初始化前和初始化后添加代码，可以加入“Bean 后处理器”



Bean 后处理器实现步骤

第一步：编写一个类，实现 BeanPostProcessor 类，重写 before 和 after 方法

```java
public class LogBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("Bean 后处理器的 before 方法执行，即将开始初始化");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("Bean 后处理器的 after 方法执行，已完成初始化");
        return bean;
    }
}
```

第二步：在 spring 配置文件中配置

```xml
<!-- 配置Bean后处理器，这个后处理器将作用于当前配置文件中所有的 bean -->
<bean class="com.shameyang.spring6.bean.LogBeanPostProcessor"/>
```



## 8.3 Bean 的生命周期十步

对 Bean 的生命周期更加细分，可以再增加三步，检查 Bean 是否实现了特定的接口，如果实现了，spring 容器会调用接口中的方法

第一步：实例化 Bean

第二步：Bean 属性赋值

第三步：检查是否实现了 Aware 的相关接口

Aware 相关接口

- BeanNameAware

  当实现了该接口，Spring 会传递 Bean 的名字

- BeanClassLoaderAware

  当实现了该接口，Spring 会传递 Bean 的类加载器

- BeanFactoryAware

  当实现了该接口，Spring 会传递 BeanFactory 对象

第四步：Bean 后处理器 before 方法

第五步：检查是否实现了 InitializingBean 接口

第六步：初始化 Bean

第七步：Bean 后处理器 after 方法

第八步：使用 Bean

第九步：检查是否实现了 DisposableBean 接口

第十步：销毁 Bean





示例如下：

```java
public class User implements BeanNameAware, BeanClassLoaderAware, BeanFactoryAware, InitializingBean, DisposableBean {
    private String name;

    public User() {
        System.out.println("1.实例化Bean");
    }

    public void setName(String name) {
        this.name = name;
        System.out.println("2.Bean属性赋值");
    }

    public void initBean(){
        System.out.println("6.初始化Bean");
    }

    public void destroyBean(){
        System.out.println("10.销毁Bean");
    }

    @Override
    public void setBeanClassLoader(ClassLoader classLoader) {
        System.out.println("3.类加载器：" + classLoader);
    }

    @Override
    public void setBeanFactory(BeanFactory beanFactory) throws BeansException {
        System.out.println("3.Bean工厂：" + beanFactory);
    }

    @Override
    public void setBeanName(String name) {
        System.out.println("3.bean名字：" + name);
    }

    @Override
    public void destroy() throws Exception {
        System.out.println("9.DisposableBean destroy");
    }

    @Override
    public void afterPropertiesSet() throws Exception {
        System.out.println("5.afterPropertiesSet执行");
    }
}
```

```java
public class LogBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("4.Bean后处理器的before方法执行，即将开始初始化");
        return bean;
    }

    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("7.Bean后处理器的after方法执行，已完成初始化");
        return bean;
    }
}
```



## 8.4 Bean 的作用域对生命周期管理的影响

Spring 容器只对 singleton 的 Bean 进行完整的生命周期管理

如果是 prototype 的 Bean，Spring 只负责创建，创建完毕后交给客户端代码管理



## 8.5 自己 new 的对象如何被 spring 管理

```java
public class RegisterBeanTest {
    @Test
    public void testBeanRegister(){
        // 自己 new 的对象
        User user = new User();
        System.out.println(user);

        // 创建默认可列表 BeanFactory 对象
        DefaultListableBeanFactory factory = new DefaultListableBeanFactory();
        // 注册 Bean
        factory.registerSingleton("userBean", user);
        // 从 spring 容器中获取 bean
        User userBean = factory.getBean("userBean", User.class);
        System.out.println(userBean);
    }
}
```







# 九、Spring IoC 注解式开发

注解主要为了简化 XML 的配置。Spring6倡导全注解开发



## 9.1 声明 Bean 的注解

负责声明 Bean 的注解，常见的有四个：

- @Component
- @Controller
- @Service
- @Repository

@Controller、@Service、@Repository 都是@Component 的别名

这四个注解的功能都是一样的，只是为了增强程序的可读性

- 控制器上用@Controller
- service 类上用@Service
- dao 类上用@Repository

他们都只有一个 value 属性，用来指定 bean 的 id，与之前配置文件中的 id 同理



部分源码如下：

```java
@Target(value = {ElementType.TYPE})
@Retention(value = RetentionPolicy.RUNTIME)
public @interface Component {
    String value();
}
```

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Controller {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Service {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```

```java
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {
    @AliasFor(
        annotation = Component.class
    )
    String value() default "";
}
```



## 9.2 注解的使用

第一步：加入 aop 的依赖

加入 spring-context 依赖后，会关联加入 aop 的依赖



第二步：在配置文件中添加 context 命名空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

</beans>
```

第三步：在配置文件中指定要扫描的包

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
    <context:component-scan base-package="com.shameyang.spring6.bean"/>
</beans>
```

第四步：在 Bean 类上使用注解

```java
@Component(value = "studentBean")
public class Student {
}
```



## 9.3 选择性实例化 Bean

上边提到了四个实例化 Bean 的注解，由于业务需要，我们有时只需要让某一个注解参与 Bean 管理，其他的都不实例化

有以下两种方式：use-default-filters="true|false"

第一种：use-default-filters="false"，不使用默认的实例化规则，指定实例化的注解

```xml
<context:component-scan base-package="com.shameyang.spring6.bean3" use-default-filters="false">
	<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```

第二种：use-default-filters="true"，使用默认的实例化规则，排除不参与实例化的注解

```xml
<context:component-scan base-package="com.shameyang.spring6.bean3" use-default-filters="true">
	<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
</context:component-scan>
```



## 9.4 负责注入的注解

上边的注解，只是声明了 Bean，我们还需要给其赋值。给 Bean 属性赋值需要这些注解：

- @Value
- @Autowired
- @Qualifier
- @Resource



### @Value

该注解用来注入简单类型，可以出现在属性、setter 方法以及构造参数的形参上，非常灵活

```java
@Component(value = "studentBean")
public class Student {
    @Value("001")
    private String sno;
    private String sname;

    @Override
    public String toString() {
        return "Student{" +
                "sno='" + sno + '\'' +
                ", sname='" + sname + '\'' +
                '}';
    }

    @Value("jack")
    public void setSname(String sname) {
        this.sname = sname;
    }
}
```



### @AutoWired 和 @Qualifier

注入非简单类型时，需要这两个注解

- @AutoWired 注解可以出现在：属性、构造方法、构造方法的参数、setter 方法上
- 带参的构造方法只有一个时，@AutoWired 可以省略
- 只有 @AutoWired 会根据类型注入，如果想要根据名字注入需要添加 @Qualifier 注解



### @Resource

@Resource 也可以用于注入非简单类型

它与@AutoWired 的区别：

- 通用性

  @Resource 是 JDK 扩展包中的，标准注解，更具有通用性

  @AutoWired 注解是 Spring 框架自己的

- 运用的地方不同

  @Resource 用在属性、setter 方法上

  @AutoWired 用在属性、setter 方法、构造方法和构造方法的参数上

- 默认的装配方式不同

  @Resource 默认根据名称 byName 装配

  @AutoWired 默认根据类型 byType 装配



## 9.5 全注解式开发

全注解开发，即不使用 spring 配置文件，写一个配置类来代替配置文件

```java
package com.shameyang.spring6;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

/**
 * @author ShameYang
 * @date 2023/10/13 19:03
 * @description 配置文件类代替 spring.xml
 */
@Configuration
@ComponentScan("com.shameyang.spring6.bean")
public class Spring6Configuration {
}
```

测试程序修改：不再 new ClassPathXMLApplicationContext 对象了

```java
public class IoCAnnotationTest {
    @Test
    public void testNew() {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Spring6Configuration.class);
        Student student = applicationContext.getBean("studentBean", Student.class);
        System.out.println(student);
    }
}
```







# 十、面向切面编程 AOP

## 10.1 AOP 介绍

Aspect Oriented Programming，面向切面编程，是一种编程技术

将与核心业务无关的代码抽取出来，形成一个独立的组件，将业务看作纵向，以横向交叉的方式应用到业务流程的这个过程，即 AOP

AOP 的优点：

- 代码复用性增强
- 代码易维护
- 开发者更关注业务逻辑



## 10.2 AOP 术语

连接点 Joinpoint

- 在程序的整个执行流程中，可以织入切面的位置

切点 Pointcut

- 在程序执行流程中，织入切面的方法

通知 Advice

- 又叫做增强，就是织入的代码
- 通知包括
  - 前置通知
  - 后置通知
  - 环绕通知
  - 异常通知
  - 最终通知

切面 Aspect

- 切点+通知

织入 Weaving

- 通知应用到目标对象上的过程

代理对象 Proxy

- 一个目标对象被织入通知后产生的新对象

目标对象 Target

- 被织入通知的对象



示例代码如下：

```java
public class UserService{
    public void do1(){
        System.out.println("do 1");
    }
    // 核心业务方法
    public void service(){
        try {
            // JoinPoint
            // 前置通知
            do1(); // Pointcut
			// 后置通知，前后都有就是环绕通知
        } catch(Exception e) {
            // JoinPoint
            //异常通知
        } finally {
            // JoinPoint
            // 最终通知
        }

    }
}
```



## 10.3 切点表达式

切点表达式用来定义通知往哪些方法上切入

语法格式：`execution([访问控制权限修饰符] 返回值类型 [全限定类名]方法名(形参列表) [异常])`

访问控制权限修饰符：

- 省略时表示4个权限都包括

返回值类型：

- *表示返回值类型任意

全限定类名：

- 两个点表示当前包以及子包下的所有类
- 省略时表示所有的类

方法名：

- *表示所有方法
- xxx*表示所有的xxx开始的方法

形参列表：

- （）表示无参方法
- （..）参数类型和个数随意的方法
- （*）只有一个参数的方法
- （*, String）第一个参数类型随意，第二个参数为 String

异常：

- 省略时表示任意类型的异常



示例如下：

service 包下所有的类中以 delete 开始的所有 public 方法

```java
execution(public * com.shameyang.mall.service.*.delete*(..))
```

mall 包下所有的类的所有的方法

```java
execution(* com.shameyang.mall..*(..))
```

所有类的所有方法

```java
execution(* *(..))
```



## 10.4 基于 AspectJ 的 AOP 注解式开发

第一步：配置 pom.xml 文件和 spring.xml

pom.xml 中引入依赖（spring-context 关联引入了 spring-aop）

```xml
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
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>6.1.0-M2</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13.2</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

spring.xml 中添加 context 命名空间和 aop 命名空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

</beans>
```



第二步：定义目标类和目标方法，并纳入 spring bean 管理

```java
package com.shameyang.spring6.service;

import org.springframework.stereotype.Service;

// 目标类
@Service
public class OrderService {
    // 目标方法
    public void generate() {
        System.out.println("订单已生成");
    }
}
```



第三步：定义切面类，并纳入 spring bean 管理

```java
package com.shameyang.spring6.service;

import org.aspectj.lang.annotation.Aspect;
import org.springframework.stereotype.Component;

// 切面类
@Aspect
@Component
public class MyAspect {    
}
```



第四步：spring.xml 中开启组件扫描，启用自动代理

```xml
<context:component-scan base-package="com.shameyang.spring6.service"/>
<aop:aspectj-autoproxy proxy-target-class="true"/>
```



第五步：切面类中添加通知，然后添加切点表达式

```java
@Aspect
@Component
public class MyAspect {
    @Before("execution(* com.shameyang.spring6.service.OrderService.*(..))")
    public void adviceBefore() {
        System.out.println("前置通知...");
    }

    @Around("execution(* com.shameyang.spring6.service.OrderService.*(..))")
    public void adviceAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕通知开始...");
        // 执行目标方法
        proceedingJoinPoint.proceed();
        System.out.println("环绕通知结束...");
    }
}
```



第六步：测试程序

```java
public class AOPTest {
    @Test
    public void testAOP() {
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("spring.xml");
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);
        orderService.generate();
    }
}
```



### 通知对应的注解

前置通知：@Before

后置通知：@AfterReturning

环绕通知：@Around

异常通知：@AfterThrowing

最终通知：@After



### 设置切面的优先级

可以使用@Order 注解来标识切面，为 value 指定一个整型数，数字越小，优先级越高



### 切点表达式复用

我们可以将切点表达式单独定义出来，在需要的位置引入，这样就可以实现代码复用

```java
@Aspect
@Component
public class MyAspect {
    @Pointcut("execution(* com.shameyang.spring6.service.OrderService.*(..))")
    public void pointcut() {
    }

    @Before("pointcut()")
    public void adviceBefore() {
        System.out.println("前置通知...");
    }

    @Around("pointcut()")
    public void adviceAround(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        System.out.println("环绕通知开始...");
        // 执行目标方法
        proceedingJoinPoint.proceed();
        System.out.println("环绕通知结束...");
    }
}
```



### 全注解式开发

编写一个类，代替 spring.xml 文件

```java
package com.shameyang.spring6.service;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.EnableAspectJAutoProxy;

@Configuration
@ComponentScan("com.shameyang.spring6.service")
@EnableAspectJAutoProxy(proxyTargetClass = true)
public class Spring6Configuration {
}
```



修改测试程序

```java
public class AOPTest {
    @Test
    public void testAOP() {
        ApplicationContext applicationContext = new AnnotationConfigApplicationContext(Spring6Configuration.class);
        OrderService orderService = applicationContext.getBean("orderService", OrderService.class);
        orderService.generate();
    }
}
```







# 十一、Spring 对事务的支持

## 11.1 事务管理 API

Spring 对事务的管理底层是基于 AOP 实现的，采用 AOP 的方式进行了封装

核心接口：PlatformTransactionManager，在 Spring6中的实现：

- DataSourceTransactionManager：支持 JdbcTemplate、Mybatis、Hibernate 等事务管理
- JtaTransactionManager：支持分布式事务管理



## 11.2 声明式事务基于注解的实现方式

第一步：spring.xml 中配置事务管理器

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
  <property name="dataSource" ref="dataSource"/>
</bean>
```

第二步：spring.xml 中引入 tx 命名空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
                           http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd">
```

第三步：spring.xml 中配置“事务注解驱动器”

```xml
<tx:annotation-driven transaction-manager="transactionManager"/>
```

第四步：在 service 类或方法上添加@Transactional 注解



### 11.2.1 事务中的重点属性

```java
// 事务传播行为
Propagation propagation() default Propagation.REQUIRED;
// 事务隔离级别
Isolation isolation() default Isolation.DEFAULT;
// 事务超时
int timeout() default -1;
// 只读事务
boolean readOnly() default false;
// 出现哪些异常时回滚
Class<? extends Throwable>[] rollbackFor() default {};
// 出现哪些异常时不回滚
Class<? extends Throwable>[] noRollbackFor() default {};
```

#### propagation 事务传播行为

事务传播行为用来描述某个事务传播行为修饰的方法被嵌套进另一个方法的事务时如何传播

设置事务的传播行为：`@Transaction(propagation = Propagation.REQUIRED)`

propagation 的 value 如下：

| 属性值           | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| REQUIRED（默认） | 如果当前没有事务，就新建一个事务，如果已经存在一个事务中，加入到这个事务中 |
| SUPPORTS         | 支持当前事务，如果当前没有事务，就以非事务方式执行           |
| MANDATORY        | 使用当前的事务，如果当前没有事务，就抛出异常                 |
| REQUIRES_NEW     | 新建事务，如果当前存在事务，把当前事务挂起                   |
| NOT_SUPPORTED    | 以非事务方式执行操作，如果当前存在事务，就把当前事务挂起     |
| NEVER            | 以非事务方式执行，如果当前存在事务，则抛出异常               |
| NESTED           | 如果当前存在事务，则在嵌套事务内执行。如果当前没有事务，则执行与 REQUIRED 类似的操作 |

#### isolation 事务隔离级别

设置事务的隔离级别：`@Transaction(isolation = Isolation.XXX)`

isolation 的 value 如下：

| 属性值           | 描述                                                         |
| ---------------- | ------------------------------------------------------------ |
| DEFAULT          | 不设置隔离级别                                               |
| READ_UNCOMMITTED | 读未提交。能够读取到其他事务未提交的数据（脏读）             |
| READ_COMMITTED   | 提已提交。其他事务提交以后才能读到，但是不可重复读           |
| REPEATABLE_READ  | 可重复读。只要当前事务不结束，读取到的数据一直都是一样的（幻读） |
| SERIALIZABLE     | 序列化。解决了幻读问题，事务排队执行，不支持并发             |

#### timeout 事务超时

设置事务超时时间：`@Transaction(timeout = ...s)`

默认值为-1，表示没有时间限制

当超过设置的时间，如果 DML 语句还没有执行完，则回滚

#### readOnly 只读事务

设置只读事务：`@Transaction(readOnly = true)`

启动 spring 优化策略，提高 select 语句执行效率

事务中只能执行 select 语句

#### rollbackFor

设置遇到哪些异常时回滚

例如：只有发生 RuntimeException 异常或该异常的子类异常才回滚

```java
@Transaction(rollbackFor = RuntimeException.class)
```

#### norollbackFor

设置遇到哪些异常时不回滚

例如：发生 NullPointException 或该异常的子类异常时不回滚

```java
@Transaction(norollbackFor = NullPointException.class)
```



### 11.2.2 事务的全注解式开发

编写一个类来代替配置文件

```java
@Configuration // 代替 spring.xml 文件
@ComponentScan("...") // 组件扫描
@EnableTransactionManagement // 开启事务注解
public class Spring6Config {
    @Bean(name = "dataSource")
    public DataSource getDataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.cj.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/spring6");
        dataSource.setUsername("root");
        dataSource.setPassword("123456");
        return dataSource;
    }

    @Bean(name = "jdbcTemplate")
    public JdbcTemplate getJdbcTemplate(DataSource dataSource){
        JdbcTemplate jdbcTemplate = new JdbcTemplate();
        jdbcTemplate.setDataSource(dataSource);
        return jdbcTemplate;
    }

    @Bean(name = "transactionManager")
    public DataSourceTransactionManager getDataSourceTransactionManager(DataSource dataSource){
        DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
        dataSourceTransactionManager.setDataSource(dataSource);
        return dataSourceTransactionManager;
    }

}
```



## 11.3 声明式事务基于 XML 的实现方式

第一步：添加 AspectJ 依赖

```xml
<!--aspectj依赖-->
<dependency>
  <groupId>org.springframework</groupId>
  <artifactId>spring-aspects</artifactId>
  <version>6.1.0-M2</version>
</dependency>
```



第二步：配置事务管理器

```xml
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```



第三步：配置通知

```xml
<tx:advice id="txAdvice" transaction-manager="txManager">
    <tx:attributes>
        <tx:method name="save*" propagation="REQUIRED" rollback-for="java.lang.Throwable"/>
        <tx:method name="del*" propagation="REQUIRED" rollback-for="java.lang.Throwable"/>
        <tx:method name="update*" propagation="REQUIRED" rollback-for="java.lang.Throwable"/>
        <tx:method name="transfer*" propagation="REQUIRED" rollback-for="java.lang.Throwable"/>
    </tx:attributes>
</tx:advice>
```



第四步：配置切面

```xml
<aop:config>
    <aop:pointcut id="txPointcut" expression="execution(* com.shameyang.bank.service..*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="txPointcut"/>
</aop:config>
```







# 十二、Spring 整合 JUnit

## 12.1 Spring 对 JUnit4的支持

在单元测试类上使用@RunWith 和@ContextConfiguration 之后，测试类属性上可以使用@AutoWired

相关依赖如下：

```xml
<!-- spring对junit的支持相关依赖 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>6.1.0-M2</version>
</dependency>
<!-- junit4依赖 -->
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13.2</version>
    <scope>test</scope>
</dependency>
```



单元测试类中使用相关注解：

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:spring.xml")
public class SpringJUnit4Test {
    @Autowired
    private OrderService orderService;

    @Test
    public void testConstructorDI() {
        orderService.delete();
    }
}
```



## 12.2 Spring 对 JUnit5的支持

在单元测试类上使用@ExtendWith 和@ContextConfiguration 之后，测试类属性上可以使用@AutoWired

相关依赖如下：

```xml
<!--spring对junit的支持相关依赖-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>6.1.0-M2</version>
</dependency>
<!--junit5依赖-->
<dependency>
    <groupId>org.junit.jupiter</groupId>
    <artifactId>junit-jupiter</artifactId>
    <version>5.10.0</version>
    <scope>test</scope>
</dependency>
```



单元测试类中使用相关注解：

```java
@ExtendWith(SpringExtension.class)
@ContextConfiguration("classpath:spring.xml")
public class SpringJUnit4Test {
    @Autowired
    private OrderService orderService;

    @Test
    public void testConstructorDI() {
        orderService.delete();
    }
}
```

