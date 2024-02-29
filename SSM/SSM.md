# 一、SSM 整合理解

需要两个 IoC 容器

| 容器      | 盛放的组件             |
| --------- | ---------------------- |
| web 容器  | web 相关组件           |
| root 容器 | 业务层和持久层相关组件 |



具体配置类和对应的容器

| 配置类            | 对应内容                         | 对应容器  |
| ----------------- | -------------------------------- | --------- |
| WebJavaConfig     | controller，springmvc 相关       | web 容器  |
| ServiceJavaConfig | service，tx 相关                 | root 容器 |
| MapperJavaConfig  | mapper，datasource，mybatis 相关 | root 容器 |



使用配置类进行 IoC 配置

```java
public class MyWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {

  //指定root容器对应的配置类
  //root容器的配置类
  @Override
  protected Class<?>[] getRootConfigClasses() {
    return new Class<?>[] { ServiceJavaConfig.class,MapperJavaConfig.class };
  }
  
  //指定web容器对应的配置类 webioc容器的配置类
  @Override
  protected Class<?>[] getServletConfigClasses() {
    return new Class<?>[] { WebJavaConfig.class };
  }
  
  //指定dispatcherServlet处理路径，通常为 / 
  @Override
  protected String[] getServletMappings() {
    return new String[] { "/" };
  }
}
```







# 二、SSM 整合配置

## 2.1 依赖整合

spring

- ioc/di
  - spring-context
  - jakarta.annotation-api：JSR 250 使用@Resource 注解
- aop
  - spring-aspects
- tx
  - spring-tx
  - spring-jdbc

springmvc

- spring-webmvc
- jakarta.jakartaee-web-api
- jackson-databind
- hibernate-validator / hibernate-validator-annotation-processor：参数校验注解

mybatis

- mybatis
- mysql
- pagehelper

整合需要

- spring-web：加载 spring 容器
- mybatis-spring：整合 mybatis
- druid：数据库连接池
- lombok
- logback



## 2.2 控制层配置（SpringMVC 整合）

> 主要配置 controller，springmvc 相关配置组件

```java
/**
 * @author ShameYang
 * @date 2023/11/13 11:33
 * @description controller，springmvc 相关组件配置
 * 1.实现 Springmvc 组件声明标准化接口 WebMvcConfigurer 提供了各种组件对应的方法
 * 2.添加配置类注解 @Configuration
 * 3.添加 mvc复合功能开关 @EnableWebMvc
 * 4.添加 controller 层扫描注解
 * 5.开启默认处理器,支持静态资源处理
 */
@Configuration
@EnableWebMvc
@ComponentScan("com.shameyang.controller")
public class WebJavaConfig implements WebMvcConfigurer {
    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
}
```



## 2.3 业务层配置（AOP/TX 整合）

> 主要配置 service，注解 aop 和声明事务相关配置

```java
/**
 * @author ShameYang
 * @date 2023/11/13 11:40
 * @description service，注解 aop 和声明事务相关配置
 * 1. 声明 @Configuration 注解,代表配置类
 * 2. 声明 @EnableTransactionManagement 注解,开启事务注解支持
 * 3. 声明 @EnableAspectJAutoProxy 注解,开启 aspect aop 注解支持
 * 4. 声明 @ComponentScan("com.shameyang.service") 注解,进行业务组件扫描
 * 5. 声明 transactionManager(DataSource dataSource) 方法,指定具体的事务管理器
 */
@EnableTransactionManagement
@EnableAspectJAutoProxy
@Configuration
@ComponentScan("com.shameyang.service")
public class ServiceJavaConfig {
    public DataSourceTransactionManager transactionManager(DataSource dataSource) {
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
}
```



## 2.4 持久层配置（Mybatis 整合）

> 整合思路：
>
> - 将 SqlSessionFactory 和 Mapper 实例加入到 IoC 容器
>
> - 使用 mybatis 整合包提供的 FactoryBean 快速整合

第一步：准备外部配置文件 jdbc.properties

```properties
jdbc.user=root
jdbc.password=root
jdbc.url=jdbc:mysql:///mybatis-example
jdbc.driver=com.mysql.cj.jdbc.Driver
```

第二步：整合 Mybatis

- 方式一：保留 mybatis-config.xml

  > 保留 xml 配置文件，但是数据库连接信息交给 Druid 连接池配置
  >
  > 缺点：进行 xml 文件解析，效率偏低

  mybatis 配置文件：

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration
          PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-config.dtd">
  <configuration>
      <settings>
          <!-- 开启驼峰式映射 -->
          <setting name="mapUnderscoreToCamelCase" value="true"/>
          <!-- 开启logback日志输出 -->
          <setting name="logImpl" value="SLF4J"/>
          <!-- 开启resultMap自动映射 -->
          <setting name="autoMappingBehavior" value="FULL"/>
      </settings>
  
      <typeAliases>
      	<!-- pojo类起别名 -->
          <package name="com.shameyang.pojo"/>
      </typeAliases>
  
      <plugins>
          <plugin interceptor="com.github.pagehelper.PageInterceptor">
              <property name="helperDialect" value="mysql"/>
          </plugin>
      </plugins>
  </configuration>
  ```

  mybatis 和持久层配置类

  ```java
  @Configuration
  @PropertySource("classpath:jdbc.properties")
  public class MapperJavaConfig {
  
      @Value("${jdbc.user}")
      private String user;
      @Value("${jdbc.password}")
      private String password;
      @Value("${jdbc.url}")
      private String url;
      @Value("${jdbc.driver}")
      private String driver;
  
      //数据库连接池配置
      @Bean
      public DataSource dataSource(){
          DruidDataSource dataSource = new DruidDataSource();
          dataSource.setUsername(user);
          dataSource.setPassword(password);
          dataSource.setUrl(url);
          dataSource.setDriverClassName(driver);
          return dataSource;
      }
  
      /**
       * 配置SqlSessionFactoryBean,指定连接池对象和外部配置文件即可
       * @param dataSource 需要注入连接池对象
       * @return 工厂Bean
       */
      @Bean
      public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
          //实例化SqlSessionFactory工厂
          SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
          //设置连接池
          sqlSessionFactoryBean.setDataSource(dataSource);
          //设置配置文件
          //包裹外部配置文件地址对象
          Resource resource = new ClassPathResource("mybatis-config.xml");
          sqlSessionFactoryBean.setConfigLocation(resource);
          return sqlSessionFactoryBean;
      }
  
      /**
       * 配置Mapper实例扫描工厂,配置mapper接口和mapper.xml文件所在的包
       * @return
       */
      @Bean
      public MapperScannerConfigurer mapperScannerConfigurer(){
          MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
          //设置mapper接口和xml文件所在的共同包
          mapperScannerConfigurer.setBasePackage("com.atguigu.mapper");
          return mapperScannerConfigurer;
      }
  
  }
  ```

  

- 方式二：完全配置类

  > 避免了 XML 文件解析效率低的问题

  数据库配置类

  ```java
  @Configuration
  @PropertySource("classpath:jdbc.properties")
  public class DataSourceJavaConfig {
      @Value("${jdbc.user}")
      private String user;
      @Value("${jdbc.password}")
      private String password;
      @Value("${jdbc.url}")
      private String url;
      @Value("${jdbc.driver}")
      private String driver;
  
      // 配置数据源
      @Bean
      public DataSource dataSource() {
          DruidDataSource dataSource = new DruidDataSource();
          dataSource.setUsername(user);
          dataSource.setPassword(password);
          dataSource.setUrl(url);
          dataSource.setDriverClassName(driver);
          return dataSource;
      }
  }
  ```

  mapper 配置类：

  ```java
  @Configuration
  public class MapperJavaConfig {    
      // 注入SqlSessionFactoryBean
      @Bean
      public SqlSessionFactoryBean sqlSessionFactoryBean(DataSource dataSource){
          // 实例化SqlSessionFactory工厂
          SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
          // 设置连接池
          sqlSessionFactoryBean.setDataSource(dataSource);
          // settings
          org.apache.ibatis.session.Configuration configuration = new org.apache.ibatis.session.Configuration();
          configuration.setMapUnderscoreToCamelCase(true);
          configuration.setLogImpl(Slf4jImpl.class);
          configuration.setAutoMappingBehavior(AutoMappingBehavior.FULL);
          sqlSessionFactoryBean.setConfiguration(configuration);
          // typeAliases
          sqlSessionFactoryBean.setTypeAliasesPackage("com.shameyang.pojo");
          // 分页插件配置
          PageInterceptor pageInterceptor = new PageInterceptor();
          Properties properties = new Properties();
          properties.setProperty("helperDialect","mysql");
          pageInterceptor.setProperties(properties);
          sqlSessionFactoryBean.addPlugins(pageInterceptor);
          return sqlSessionFactoryBean;
      }
  
      // 配置扫描
      @Bean
      public MapperScannerConfigurer mapperScannerConfigurer(){
          MapperScannerConfigurer mapperScannerConfigurer = new MapperScannerConfigurer();
          // 配置需要扫描的 mapper 包
          mapperScannerConfigurer.setBasePackage("com.shameyang.mapper");
          return mapperScannerConfigurer;
      }
  
  }
  ```



## 2.5 容器初始化配置类

```java
public class MyWebAppInitializer extends AbstractAnnotationConfigDispatcherServletInitializer {
    //指定root容器对应的配置类
    @Override
    protected Class<?>[] getRootConfigClasses() {
        return new Class<?>[]{MapperJavaConfig.class, ServiceJavaConfig.class, DataSourceJavaConfig.class};
    }

    //指定web容器对应的配置类
    @Override
    protected Class<?>[] getServletConfigClasses() {
        return new Class<?>[]{WebJavaConfig.class};
    }

    //指定dispatcherServlet处理路径，通常为 / 
    @Override
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

