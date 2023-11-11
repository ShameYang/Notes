# 前言

参考视频：[b站尚硅谷赵伟风](https://www.bilibili.com/video/BV1AP411s7D7?p=124&spm_id_from=pageDriver&vd_source=ca277c9be3ac177cc2c26f2f391d0252)

参考笔记：[springmvc参考笔记](https://www.wolai.com/mY4orG21749UeVBHsefaAb)



# 一、SpringMVC 简介

## 1.1 什么是 MVC

MVC 是一种软件架构的思想，将软件划分为模型、视图、控制器

M：Model，模型层，指工程中的 JavaBean，负责处理数据

- JavaBean 分为两类：
  - 实体类 Bean：负责存储业务数据
  - 业务处理 Bean：Servlet 或 Dao 对象，用于处理业务逻辑和数据访问

V：View，视图层，指工程中的 html 或 jsp 页面，负责与用户交互，展示数据

C：Controller，控制层，指工程中的 servlet，负责接收请求和响应浏览器



## 1.2 什么是 SpringMVC

SpringMVC 是 Spring 的一个模块，基于 MVC 开发模式的框架，用来优化控制层，专门用来做 web 开发

web 开发的底层是 servlet，框架是在 servlet 基础上加入了一些功能，使开发更加方便

SpringMVC 容器中存放控制器对象，控制器对象可以接收用户的请求，当作一个 servlet 使用

SpringMVC 中的 Servlet 对象：DispatcherServlet（中央调度器），负责接收用户的所有请求，之后把请求转发给 Controller 对象，由 Controller 对象处理请求



## 1.3 SpringMVC 框架的特点

① 轻量级，简单易学

② 高效 , 基于请求响应的 MVC 框架

③ 与 Spring 兼容性好，无缝结合

④ 约定优于配置

⑤ 功能强大：RESTful、数据验证、格式化、本地化、主题等

⑥ 简洁灵活







# 二、第一个 SpringMVC 程序

**实现步骤**

① 引入依赖

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
        <groupId>jakarta.servlet</groupId>
        <artifactId>jakarta.servlet-api</artifactId>
        <version>6.0.0</version>
        <scope>provided</scope>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-webmvc</artifactId>
        <version>6.1.0-M2</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>3.8.1</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

② 声明 Controller

```java
@Controller
public class HelloController {
    @RequestMapping("springmvc/hello") // 对外访问的地址
    @ResponseBody // 直接返回字符串给前端，不找视图解析器
    public String hello() {
        System.out.println("Hello");
        // 返回给前端
        return "hello springmvc!";
    }
}
```

③ 编写配置类 MvcConfig，代替 spring.xml

```java
/**
 * @author ShameYang
 * @date 2023/11/7 15:41
 * @description
 *  1.controller配置ioc容器
 *  2.handlerMapping handlerAdapter加入到ioc容器
 */
@Configuration
@ComponentScan("com.shameyang.controller")
public class MvcConfig {
    @Bean
    public RequestMappingHandlerMapping handlerMapping() {
        return new RequestMappingHandlerMapping();
    }

    @Bean
    public RequestMappingHandlerAdapter handlerAdapter() {
        return new RequestMappingHandlerAdapter();
    }
}
```

④ 编写 SpringMvcInit，代替 web.xml

```java
/**
 * @author ShameYang
 * @date 2023/11/7 15:47
 * @description 可以被web项目加载，会初始化ioc容器，会设置dispatcherServlet的地址
 */
public class SpringMvcInit extends AbstractAnnotationConfigDispatcherServletInitializer {
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class[0];
    }

    // 设置项目对应的配置
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{MvcConfig.class};
    }

    // 配置springmvc内部自带servlet的访问地址
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

⑤ 部署到 Tomcat 服务器运行







# 三、SpringMVC 接收数据

## 3.1 访问路径设置

@RequestMapping 注解的作用：将请求的 URL 地址和处理请求的方式（handler 方法）关联起来，建立映射关系

SpringMVC 接收到指定的请求后，就会在映射关系中找到对应的方法来处理请求

① 精确路径匹配

```java
public class User {
	@RequestMapping("/user/login")
    public void login() {
        ...
    }
}
```

② 模糊匹配

一层用 *，多层用 **

```java
public class User {
	@RequestMapping("/user/*")
    public void login() {
        ...
    }
}
```

③ 类上添加@RequestMapping

设置通用的地址

访问：类地址 + 方法地址

```java
@RequestMapping("user")
public class User {
    @RequestMapping("login")
    public void login() {
        ...
    }
    
    @RequestMapping("register")
    public void register() {
        ...
    }
}
```

④ 指定请求方式

```java
public class User {
    @RequestMapping(value = "/user/login", method = "RequestMethod.xxx")
    public void login() {
        ...
    }
}
// 下边为注解指定请求方式
public class User {
    @RequestMapping("/user/login")
    @PostMapping // = @RequestMapping(..., method = "RequestMethod.POST")
    public void login() {
        ...
    }
}
```



## 3.2 接收参数

### 3.2.1 param 和 json 参数比较

param 类型的参数适合单一的数据传递，JSON 类型的参数更适合复杂的数据结构传递。根据具体业务需求选择。实际开发中，常见的做法：GET 请求中采用 param 类型的参数，POST 请求中采用 JSON 类型的参数传递



### 3.2.2 param 参数接收

直接接收

```java
@RequestMapping("param")
public class ParamController {
    // 直接接收
    // /param/data?name=root&age=18
    // 形参列表填写对应名称的参数即可，可以不传递，不会报错
    @RequestMapping("data")
    @ResponseBody
    public String data(String name, int age) {
        return "name=" + name + ",age=" + age;
    }
}
```

注解指定

@RequestParam(value = "指定请求参数名，形参名和请求参数名一致，可以省略"

​								required = false | true 前端是否必须传递此参数，默认为 true，不传400异常

​								defaultValue = "非必须传递时，可以设置默认值")

```java
@RequestMapping("param")
public class ParamController {
	// 注解指定
    // /param/data1?account=root&page=1
    @GetMapping("data1")
    @ResponseBody
    public String data1(@RequestParam(value = "account") String username,
                        @RequestParam(required = false, defaultValue = "1") int page)
        return "username=" + username + ",page=" + page;
}
```

接收集合

```java
@RequestMapping("param")
public class ParamController {
	// 一名多值 key=1&key=2
    // /param/data2?hbs=sing&hbs=jump&hbs=rap
    @GetMapping("data2")
    @ResponseBody
    public String data2(@RequestParam List<String> hbs) {
        return "ok";
    }
}
```

实体对象接值

定义一个 pojo 类，属性名与参数名一致，使用@Data 注解

```java
@RequestMapping("param")
public class ParamController {
	// /param/data3?name=zhangsan&age=18
    @RequestMapping("data3")
    @ResponseBody
    public String data3(User user) {
        return user.toString();
    }
}
```



### 3.2.3 路径参数接收

在 URL 路径中传递参数，SpringMVC 中提供了@PathVariable 注解来处理路径传递参数，可以让路径变短

1.使用 {} 设置动态路径

2.使用@PathVariable 接收动态路径参数

示例：将/user/{id}/{username} 中的 {id}和{username} 映射到控制器方法的一个参数中

```java
@RequestMapping("param")
public class ParamController{
	// /param/user/1/root -> id=1 username=root
	@GetMapping("/user/{id}/{username}")
    @ResponseBody
    public String getUser(@PathVariable Long id,
                          @PathVariable("username") String username) {
        return "id=" + id + ",username=" + username;
    }
}
```



### 3.2.4 json 参数接收

需要使用@RequestBody 注解接收 JSON 参数

```java
@RequestMapping("json")
@Controller
@ResponseBody
public class JsonController {
    @PostMapping("data")
    public String data(@RequestBody Person person) {
        return person.toString();
    }
}
```

Java 原生 API 只支持路径参数和 param 参数，前端会报 415 异常

解决办法：

- 引入 json 处理依赖（jackson-databind）

  ```xml
  <dependency>
  	<groupId>com.fasterxml.jackson.core</groupId>
  	<artifactId>jackson-databind</artifactId>
  	<version>2.15.2</version>
  </dependency>
  ```

- handlerAdapter 配置 json 转化器（配置类上使用@EnableWebMvc 注解）

  - @EnableWebMvc 等价于在 XML 配置中使用 \<mvc:annotation-driven>

  ```java
  @EnableWebMvc
  @Configuration
  @ComponentScan("com.shameyang.controller")
  public class MvcConfig {
      @Bean
      public RequestMappingHandlerMapping handlerMapping() {
          return new RequestMappingHandlerMapping();
      }
  
      @Bean
      public RequestMappingHandlerAdapter handlerAdapter() {
          return new RequestMappingHandlerAdapter();
      }
  }
  ```





## 3.3 接收 Cookie 数据

可以使用@CookieValue 注解将 Cookie 的值绑定到控制器中的方法参数

```java
@GetMapping("/demo")
public void handle(@CookieValue("JSESSIONID") String cookie) { 
	...
}
```



## 3.4 接收请求头数据

可以使用@RequestHeader 注解将请求标头绑定到控制器中的方法参数

带有标头的请求：

```
Host                    localhost:8080
Accept                  text/html,application/xhtml+xml,application/xml;q=0.9
Accept-Language         fr,en-gb;q=0.7,en;q=0.3
Accept-Encoding         gzip,deflate
Accept-Charset          ISO-8859-1,utf-8;q=0.7,*;q=0.7
Keep-Alive              300
```

下面示例：获取 Host 标头的值

```java
@GetMapping("/demo")
public void handle(@RequestHeader("Host") String host) { 
	...
}
```







# 四、SpringMVC 响应数据

## 4.1 页面跳转控制

### 4.1.1 快速返回模板视图

第一步：准备 jsp 页面和依赖

> pom.xml 依赖
>
> ```xml
> <dependency>
>     <groupId>jakarta.servlet.jsp.jstl</groupId>
>     <artifactId>jakarta.servlet.jsp.jstl-api</artifactId>
>     <version>3.0.0</version>
> </dependency>
> ```
>
> jsp 页面
>
> 建议位置：/WEB-INF/ 下，避免外部直接访问
>
> ```jsp
> <%@ page contentType="text/html;charset=UTF-8" language="java" %>
> <html>
>   <head>
>     <title>Title</title>
>   </head>
>   <body>
>         <!-- request.setAttribute("data", "hello jsp!"); -->
>         ${data}
>   </body>
> </html>
> 
> ```

第二步：快速响应模板页面

> 配置 jsp 视图解析器
>
> ```java
> @EnableWebMvc
> @Configuration
> @ComponentScan("com.shameyang.controller")
> public class SpringMvcConfig implements WebMvcConfigurer {
>     //配置jsp对应的视图解析器
>     @Override
>     public void configureViewResolvers(ViewResolverRegistry registry) {
>         //快速配置jsp模板语言对应的
>         registry.jsp("/WEB-INF/views/",".jsp");
>     }
> }
> ```
>
> handler 返回视图
>
> ```java
> @Controller
> @RequestMapping("jsp")
> public class JspController {
>     /**
>      *	快速查找视图
>      *	1.方法返回字符串类型
>      *	2.不能添加@ResponseBody
>      *	3.返回值对应jsp文件名称
>      */
>     @GetMapping("index")
> 	public String jumpJsp(HttpServletRequest request){
>     	request.addAttribute("data","hello jsp!");
>     	return "index";
> 	}
> }
> ```



### 4.1.2 转发和重定向

SpringMVC 中，Handler 方法返回值可以实现快速转发，使用 **redirect** 或 **forward** 关键字

- 转发使用 forward 关键字，重定向使用 redirect 关键字
- 关键字:/路径
- 注意：如果是项目下的资源，转发和重定向都是项目下路径！都不需要添加项目根路径！

```java
@RequestMapping("/forward-demo")
public String forwardDemo() {
    // 转发到 /demo 路径
    return "forward:/demo";
}

@RequestMapping("/redirect-demo")
public String redirectDemo() {
    // 重定向到 /demo 路径 
    return "redirect:/demo";
}
```



## 4.2 返回 JSON 数据

### 4.2.1 前置准备

引入 jackson 依赖

```xml
<dependency>
	<groupId>com.fasterxml.jackson.core</groupId>
	<artifactId>jackson-databind</artifactId>
	<version>2.15.2</version>
</dependency>
```

添加 json 数据转化器（@EnableWebMvc）

```java
@EnableWebMvc
@Configuration
@ComponentScan("com.shameyang.controller")
public class MvcConfig {
    @Bean
    public RequestMappingHandlerMapping handlerMapping() {
        return new RequestMappingHandlerMapping();
    }

    @Bean
    public RequestMappingHandlerAdapter handlerAdapter() {
        return new RequestMappingHandlerAdapter();
    }
}
```



### 4.2.2 @ResponseBody

我们可以在方法或类上使用@ResponseBody 注解，表示方法的返回值是直接返回到客户端的数据，不需要经过视图解析器

这样就可以直接返回 JSON 格式的数据了



### 4.2.3 @RestController

@RestController 注解可以用在类上，是@Controller 和@ResponseBody 的结合

源码如下：

```java
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Controller
@ResponseBody
public @interface RestController {
 
  /**
   * The value may indicate a suggestion for a logical component name,
   * to be turned into a Spring bean in case of an autodetected component.
   * @return the suggested component name, if any (or empty String otherwise)
   * @since 4.0.1
   */
  @AliasFor(annotation = Controller.class)
  String value() default "";
 
}
```



## 4.3 返回静态资源

1.什么是静态资源

> 资源本身已经是可以直接拿到浏览器上使用的程度了，**不需要在服务器端做任何运算、处理**。典型的静态资源包括：
>
> - 纯 HTML 文件
> - 图片
> - CSS 文件
> - JavaScript 文件
> - ……

2.静态资源访问

> 问题分析
> - DispatcherServlet 的 url-pattern 配置的是 “/”
> - url-pattern 配置 “/” 表示整个 Web 应用范围内所有请求都由 SpringMVC 来处理
> - 对 SpringMVC 来说，必须有对应的 @RequestMapping 才能找到处理请求的方法
> - 现在 images/mi.jpg 请求没有对应的 @RequestMapping 所以返回 404
>
> 问题解决
>
> 在 SpringMVC 配置类中配置
>
> ```java
> @EnableWebMvc
> @Configuration
> @ComponentScan("com.shameyang.controller")
> public class SpringMvcConfig implements WebMvcConfigurer { 
>     //开启静态资源处理 <mvc:default-servlet-handler/>
>     @Override
>     public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
>         configurer.enable();
>     }
> }
> ```







# 五、RESTFul 风格

## 5.1 RESTFul 风格概述

### 5.1.1 RESTFul 简介

RESTFul 是一种设计风格，简单来说，提供了 HTTP 协议的最标准的使用方案

> URL：如何设计请求路径
>
> 请求方式：如何选择请求方式
>
> param：如何选择参数传递方式

学习 RESTFul 原则，可以帮助我们更好的设计 HTTP 协议的 API 接口



### 5.1.2 RESTFul 特点

1. 每一个 URI 代表1种资源（URI 是名词）
2. 客户端使用 GET、POST、PUT、DELETE 4个表示操作方式的动词对服务端资源进行操作：GET 用来获取资源，POST 用来新建资源（也可以用于更新资源），PUT 用来更新资源，DELETE 用来删除资源
3. 资源的表现形式是 XML 或者 JSON
4. 客户端与服务端之间的交互在请求之间是无状态的，从客户端到服务端的每个请求都必须包含理解请求所必需的信息



### 5.1.3 **RESTFul风格设计规范**
  1. **HTTP协议请求方式要求**

      REST 风格主张在项目设计、开发过程中，具体的操作符合**HTTP协议定义的请求方式的语义**。

  2. **URL路径风格要求**

      > REST风格下每个资源都应该有一个唯一的标识符，例如一个 URI（统一资源标识符）或者一个 URL（统一资源定位符）。资源的标识符应该能明确地说明该资源的信息，同时也应该是可被理解和解释的！
      >
      > 使用URL+请求方式确定具体的动作，他也是一种标准的HTTP协议请求！
      >
      > | 操作 | 传统风格                | REST 风格                              |
      > | ---- | ----------------------- | -------------------------------------- |
      > | 保存 | /CRUD/saveEmp           | URL 地址：/CRUD/emp 请求方式：POST     |
      > | 删除 | /CRUD/removeEmp?empId=2 | URL 地址：/CRUD/emp/2 请求方式：DELETE |
      > | 更新 | /CRUD/updateEmp         | URL 地址：/CRUD/emp 请求方式：PUT      |
      > | 查询 | /CRUD/editEmp?empId=2   | URL 地址：/CRUD/emp/2 请求方式：GET    |

  - 总结

      根据接口的具体动作，选择具体的HTTP协议请求方式

      路径设计从原来携带动标识，改成名词，对应资源的唯一标识即可！



### 5.1.4 RESTFul风格好处

  1. 含蓄，安全

      > 使用问号键值对的方式给服务器传递数据太明显，容易被人利用来对系统进行破坏。使用 REST 风格携带数据不再需要明显的暴露数据的名称
      
  2. 风格统一

      > URL 地址整体格式统一，从前到后始终都使用斜杠划分各个单词，用简单一致的格式表达语义
      
  3. 无状态

      > 在调用一个接口（访问、操作资源）的时候，可以不用考虑上下文，不用考虑当前状态，极大的降低了系统设计的复杂度
      
  4. 严谨，规范

      > 严格按照 HTTP1.1 协议中定义的请求方式本身的语义进行操作
      
  5. 简洁，优雅

      > 过去做增删改查操作需要设计4个不同的URL，现在一个就够了
      >
      > | 操作 | 传统风格                | REST 风格                              |
      > | ---- | ----------------------- | -------------------------------------- |
      > | 保存 | /CRUD/saveEmp           | URL 地址：/CRUD/emp 请求方式：POST     |
      > | 删除 | /CRUD/removeEmp?empId=2 | URL 地址：/CRUD/emp/2 请求方式：DELETE |
      > | 更新 | /CRUD/updateEmp         | URL 地址：/CRUD/emp 请求方式：PUT      |
      > | 查询 | /CRUD/editEmp?empId=2   | URL 地址：/CRUD/emp/2 请求方式：GET    |

  6. 丰富的语义

      > 通过 URL 地址就可以知道资源之间的关系。它能够把一句话中的很多单词用斜杠连起来，反过来说就是可以在 URL 地址中用一句话来充分表达语义。
      >
      > [http://localhost:8080/shop](http://localhost:8080/shop) [http://localhost:8080/shop/product](http://localhost:8080/shop/product) [http://localhost:8080/shop/product/cellPhone](http://localhost:8080/shop/product/cellPhone) [http://localhost:8080/shop/product/cellPhone/iPhone](http://localhost:8080/shop/product/cellPhone/iPhone)



## 5.2 RESTFul 风格实战

### 5.2.1 需求分析

- 数据结构： User {id 唯一标识,name 用户名，age 用户年龄}
- 功能分析
    - 用户数据分页展示功能（条件：page 页数 默认1，size 每页数量 默认 10）
    - 保存用户功能
    - 根据用户id查询用户详情功能
    - 根据用户id更新用户数据功能
    - 根据用户id删除用户数据功能
    - 多条件模糊查询用户功能（条件：keyword 模糊关键字，page 页数 默认1，size 每页数量 默认 10）



### 5.2.2 RESTFul 风格接口设计

| 功能     | 接口和请求方式   | 请求参数                      | 返回值       |
| -------- | ---------------- | ----------------------------- | ------------ |
| 分页查询 | GET  /user       | page=1&size=10                | { 响应数据 } |
| 用户添加 | POST /user       | { user 数据 }                 | {响应数据}   |
| 用户详情 | GET /user/1      | 路径参数                      | {响应数据}   |
| 用户更新 | PUT /user        | { user 更新数据}              | {响应数据}   |
| 用户删除 | DELETE /user/1   | 路径参数                      | {响应数据}   |
| 条件模糊 | GET /user/search | page=1&size=10&keywork=关键字 | {响应数据}   |



### 5.2.3 后台接口实现

pojo 类

```java
@Data
public class User {
    private Integer id;
    private String name;
    private Integer age;
}
```

Controller

```java
@RestController
@RequestMapping("user")
public class UserController {
    @GetMapping
    public void queryPage(@RequestParam(required = false, defaultValue = "1") int page,
                          @RequestParam(required = false, defaultValue = "10") int size) {
    }

    @PostMapping
    public void add(@RequestBody User user) {
    }

    @GetMapping
    public User selectById(@PathVariable int id) {
        return null;
    }

    @PutMapping
    public void update(@RequestBody User user) {
    }

    @DeleteMapping
    public void deleteById(@PathVariable int id) {
    }

    public List<User> queryPage(@RequestParam(required = false, defaultValue = "1") int page, 
                                @RequestParam(required = false, defaultValue = "10") int size, 
                                String keyword) {
        return null;
    }
}
```







# 六、SpringMVC 其他扩展

## 6.1 全局异常处理

### 6.1.1 异常处理两种方式

对于异常处理，一般分为两种方式：

- 编程式异常处理

  > 在代码中显示地编写处理异常的逻辑，try-catch

- 声明式异常处理

  > 将异常处理的逻辑从具体的业务逻辑中分离出来，通过配置等方式进行统一管理
  >
  > 相较于编程式异常处理，声明式异常处理可以使代码更加简洁、易于维护和扩展



### 6.1.2 基于注解声明式异常处理

1. 声明异常处理控制器类

   > 控制器类上使用@ControllerAdvice 或 @RestControllerAdvice 注解

   ```java
   /**
    * @ControllerAdvice 代表当前类的异常处理controller! 
    * @RestControllerAdvice = @ControllerAdvice + @ResponseBody
    */
   @RestControllerAdvice
   public class GlobalExceptionHandler {
   }
   ```

2. 声明异常处理 handler 方法

   > 方法上使用@ExceptionHandler 注解
   >
   > 具体的异常处理 Handler 优先级更高

   ```java
   /**
    * 当发生空指针异常会触发此方法!
    * @param e
    * @return
    */
   @ExceptionHandler(NullPointerException.class)
   public Object handlerNullException(NullPointerException e){
   
       return null;
   }
   
   /**
    * 所有异常都会触发此方法!但是如果有具体的异常处理Handler! 
    * 具体异常处理Handler优先级更高!
    * 例如: 发生NullPointerException异常!
    *       会触发handlerNullException方法,不会触发handlerException方法!
    */
   @ExceptionHandler(Exception.class)
   public Object handlerException(Exception e){
   
       return null;
   }
   ```

3. 配置类中扫描处理处理控制器类

   ```java
   @Configuration
   @ComponentScan(basePackages = {"com.shameyang.controller","com.shameyang.exceptionhandler"})
   public class SpringMvcConfig {
   }
   ```





## 6.2 拦截器

### 6.2.1 拦截器概念

拦截器类似于过滤器，先把请求拦住，才能执行后续操作

拦截器 VS 过滤器

- 相似点

  - 拦截：必须先把请求拦住，才能执行后续操作
  - 过滤：拦截器或过滤器存在的意义就是对请求进行统一处理
  - 放行：对请求执行了必要操作后，放请求过去，让它访问原本想要访问的资源

- 不同点

  - 工作平台不同

    - 过滤器：工作在 Servlet 容器中

    - 拦截器：工作在 SpringMVC 的基础上

  - 拦截范围不同

    - 过滤器：拦截到的最大范围是整个 Web 应用
    - 过滤器：拦截到的最大范围是整个 SpringMVC 负责的请求

  - IOC 容器支持

    - 过滤器：想得到 IOC 容器需要调用专门的工具方法，是间接的
    - 拦截器：它自己就在 IOC 容器中，所以可以直接从 IOC 容器中装配组件，也就是可以直接得到 IOC 容器的支持

选择：如果用 SpringMVC 的拦截器能实现，就不用过滤器



### 6.2.2 拦截器使用

1. 创建拦截器类

   ```java
   public class MyInterceptor implements HandlerInterceptor {
       // 在处理请求的目标 handler 方法前执行
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           // 返回true：放行
           // 返回false：不放行
           return true;
       }
    
       // 在目标 handler 方法之后，handler报错不执行!
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
       }
    
       // 渲染视图之后执行(最后),一定执行!
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
       }
   }
   ```
   
2. 配置类中添加拦截器

   ```java
   @Configuration
   @ComponentScan(basePackages = {"..."})
   public class SpringMvcConfig implements WebMvcConfigurer {
       //添加拦截器
       @Override
       public void addInterceptors(InterceptorRegistry registry) { 
           //将拦截器添加到Springmvc环境,默认拦截所有Springmvc分发的请求
           registry.addInterceptor(new MyInterceptor());
       }
   }
   ```

3. 配置详解

   a.拦截全部请求

   ```java
   registry.addInterceptor(new MyInterceptor());
   ```

   b.拦截指定地址

   ```java
   registry.addInterceptor(new MyInterceptor())
       .addPathPatterns("要拦截的地址");
   ```

   c.排除拦截

   > 注意：排除的地址要在拦截地址内部！

   ```java
   registry.addInterceptor(new MyInterceptor())
       .addPathPatterns("/user/**")
       .excludePathPatterns("/user/data");
   ```

4. 多个拦截器执行顺序

   a.preHandler()：SpringMVC 会把所有拦截器收集到一起，然后按照配置顺序调用各个 preHandle() 方法

   b.postHandler()：SpringMVC 会把所有拦截器收集到一起，然后按照配置相反的顺序调用各个 postHandle() 方法

   c.afterHandler()：SpringMVC 会把所有拦截器收集到一起，然后按照配置相反的顺序调用各个 afterCompletion() 方法





## 6.3 参数校验

> 在 Web 应用三层架构体系中，表述层负责接收浏览器提交的数据，业务逻辑层负责数据的处理。为了能够让业务逻辑层基于正确的数据进行处理，我们需要在表述层对数据进行检查，将错误的数据隔绝在业务逻辑层之外

1. **校验概述**

    JSR 303 是 Java 为 Bean 数据合法性校验提供的标准框架，它已经包含在 JavaEE 6.0 标准中。JSR 303 通过在 Bean 属性上标注类似于 @NotNull、@Max 等标准的注解指定校验规则，并通过标准的验证接口对Bean进行验证
    
    | 注解                       | 规则                                           |
    | -------------------------- | ---------------------------------------------- |
    | @Null                      | 标注值必须为 null                              |
    | @NotNull                   | 标注值不可为 null                              |
    | @AssertTrue                | 标注值必须为 true                              |
    | @AssertFalse               | 标注值必须为 false                             |
    | @Min(value)                | 标注值必须大于或等于 value                     |
    | @Max(value)                | 标注值必须小于或等于 value                     |
    | @DecimalMin(value)         | 标注值必须大于或等于 value                     |
    | @DecimalMax(value)         | 标注值必须小于或等于 value                     |
    | @Size(max,min)             | 标注值大小必须在 max 和 min 限定的范围内       |
    | @Digits(integer,fratction) | 标注值值必须是一个数字，且必须在可接受的范围内 |
    | @Past                      | 标注值只能用于日期型，且必须是过去的日期       |
    | @Future                    | 标注值只能用于日期型，且必须是将来的日期       |
    | @Pattern(value)            | 标注值必须符合指定的正则表达式                 |
    
    JSR 303 只是一套标准，需要提供其实现才可以使用。Hibernate Validator 是 JSR 303 的一个参考实现，除支持所有标准的校验注解外，它还支持以下的扩展注解：
    
    | 注解      | 规则                               |
    | --------- | ---------------------------------- |
    | @Email    | 标注值必须是格式正确的 Email 地址  |
    | @Length   | 标注值字符串大小必须在指定的范围内 |
    | @NotEmpty | 标注值字符串不能是空字符串         |
    | @Range    | 标注值必须在指定的范围内           |
    
    Spring 4.0 版本已经拥有自己独立的数据校验框架，同时支持 JSR 303 标准的校验框架。Spring 在进行数据绑定时，可同时调用校验框架完成数据校验工作。在 SpringMVC 中，可直接通过注解驱动 @EnableWebMvc 的方式进行数据校验。Spring 的 LocalValidatorFactoryBean 既实现了 Spring 的 Validator 接口，也实现了 JSR 303 的 Validator 接口。只要在Spring容器中定义了一个 LocalValidatorFactoryBean，即可将其注入到需要数据校验的 Bean中。Spring本身并没有提供 JSR 303的实现，所以必须将 JSR 303的实现者的 jar 包放到类路径下。
    
    配置 @EnableWebMvc后，SpringMVC 会默认装配好一个 LocalValidatorFactoryBean，通过在处理方法的入参上标注 @Validated 注解即可让 SpringMVC 在完成数据绑定后执行数据校验的工作。

2. **操作演示**
    
    导入依赖
    
    ```xml
    <!-- 校验注解 -->
    <dependency>
        <groupId>jakarta.platform</groupId>
        <artifactId>jakarta.jakartaee-web-api</artifactId>
        <version>9.1.0</version>
        <scope>provided</scope>
    </dependency>
            
    <!-- 校验注解实现-->        
    <!-- https://mvnrepository.com/artifact/org.hibernate.validator/hibernate-validator -->
    <dependency>
        <groupId>org.hibernate.validator</groupId>
        <artifactId>hibernate-validator</artifactId>
        <version>8.0.0.Final</version>
    </dependency>
    <!-- https://mvnrepository.com/artifact/org.hibernate.validator/hibernate-validator-annotation-processor -->
    <dependency>
        <groupId>org.hibernate.validator</groupId>
        <artifactId>hibernate-validator-annotation-processor</artifactId>
        <version>8.0.0.Final</version>
    </dependency>
    ```
    
    应用校验注解
    
    ```java
    public class User {
        //age >= 10
        @Min(10)
        private int age;
    
        //name 3 <= name.length <= 6
        @Length(min = 3,max = 6)
        private String name;
    
        //email 邮箱格式
        @Email
        private String email;
    	
        //getter、setter...
    }
    
    ```
    
    handler 标记和绑定错误收集
    
    ```java
    @RestController
    @RequestMapping("user")
    public class UserController {
    
        /**
         * @Validated 代表应用校验注解! 必须添加!
         */
        @PostMapping("save")
        //在实体类参数和 BindingResult 之间不能有任何其他参数, BindingResult可以接受错误信息,避免信息抛出!
        public Object save(@Validated @RequestBody User user, BindingResult result){
           //判断是否有信息绑定错误! 有可以自行处理!
            if (result.hasErrors()){
                System.out.println("错误");
                String errorMsg = result.getFieldError().toString();
                return errorMsg;
            }
            //没有,正常处理业务即可
            System.out.println("正常");
            return user;
        }
    }
    ```

3. **易混总结**

    @NotNull、@NotEmpty、@NotBlank 都是用于在数据校验中检查字段值是否为空的注解，但是它们的用法和校验规则有所不同

    1. @NotNull（包装类型不为 null）

        @NotNull 注解是 JSR 303 规范中定义的注解，当被标注的字段值为 null 时，会认为校验失败而抛出异常。该注解不能用于字符串类型的校验，若要对字符串进行校验，应该使用 @NotBlank 或 @NotEmpty 注解
    2. @NotEmpty（集合类型长度大于0）

        @NotEmpty 注解同样是 JSR 303 规范中定义的注解，对于 CharSequence、Collection、Map 或者数组对象类型的属性进行校验，校验时会检查该属性是否为 Null 或者 size()==0，如果是的话就会校验失败。但是对于其他类型的属性，该注解无效。需要注意的是只校验空格前后的字符串，如果该字符串中间只有空格，不会被认为是空字符串，校验不会失败
    3. @NotBlank（字符串，不为null，且不为"  "字符串）

        @NotBlank 注解是 Hibernate Validator 附加的注解，对于字符串类型的属性进行校验，校验时会检查该属性是否为 Null 或 “” 或者只包含空格，如果是的话就会校验失败。需要注意的是，@NotBlank 注解只能用于字符串类型的校验

    总之，这三种注解都是用于校验字段值是否为空的注解，但是其校验规则和用法有所不同。在进行数据校验时，需要根据具体情况选择合适的注解进行校验







# SpringMVC 总结

| 核心点           | 掌握目标                                       |
| ---------------- | ---------------------------------------------- |
| springmvc框架    | 主要作用、核心组件、调用流程                   |
| **简化参数接收** | **路径设计、参数接收、请求头接收、cookie接收** |
| **简化数据响应** | **模板页面、转发和重定向、JSON数据、静态资源** |
| restful风格设计  | 主要作用、具体规范、请求方式和请求参数选择     |
| 功能扩展         | 全局异常处理、拦截器、参数校验注解             |
