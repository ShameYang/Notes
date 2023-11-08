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

定义一个 pojo 类，属性名与参数名一致，提供 get、set 方法

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

