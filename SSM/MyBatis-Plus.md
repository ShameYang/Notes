# 一、Mybatis-Plus 介绍

MyBatis-Plus（简称 MP）是一个 MyBatis 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生

主要作用：

- 自动生成单表的 CRUD 功能
- 提供丰富的条件拼接方式
- 全自动 ORM 类型持久层框架







# 二、快速入门

## 2.1 案例

第一步：准备数据库表

第二步：创建 boot 工程

第三步：引入依赖

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
    <artifactId>mp-001-quick</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>
        <dependency>
            <!-- 测试环境 -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
        </dependency>
        <dependency>
            <!-- mybatis-plus -->
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.3.2</version>
        </dependency>
        <dependency>
            <!-- 数据库相关配置启动器 -->
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
        </dependency>
        <dependency>
            <!-- druid启动器 -->
            <groupId>com.alibaba</groupId>
            <artifactId>druid-spring-boot-3-starter</artifactId>
            <version>1.2.20</version>
        </dependency>
        <dependency>
            <!-- 驱动类 -->
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
        </dependency>
    </dependencies>
    <!-- 解决spring-boot-starter的漏洞 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.yaml</groupId>
                <artifactId>snakeyaml</artifactId>
                <version>2.0</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

第四步：编写配置文件和启动类

```yaml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    druid:
      url: jdbc:mysql://xxx
      username: root
      password: xxx
      driver-class-name: com.mysql.cj.jdbc.Driver
```

```java
@MapperScan("com.shameyang.mapper")
@SpringBootApplication
public class Main {
    public static void main(String[] args) {
        SpringApplication.run(Main.class, args);
    }
}
```

第五步：实体类和 mapper 接口

```java
@Data
public class Student {
    private String sno;
    private String sname;
}
```

```java
public interface StuMapper extends BaseMapper<Student> {
    
}
```

继承 mybatis-plus 提供的基础 Mapper 接口，自带 crud 方法

第六步：测试和使用

```java
@SpringBootTest
public class MybatisPlusTest {
    @Resource
    private StuMapper stuMapper;

    @Test
    public void testSelect() {
        System.out.println("--- selectAll method test ---");
        List<Student> studentList = stuMapper.selectList(null);
        studentList.forEach(System.out::println);
    }
}
```



## 2.2 总结

- 集成 MyBatis-Plus 非常简单，只需引入 starter 工程，配置 mapper 扫描路径即可
- mapper 接口继承 MP 提供的 BaseMapper 接口，自带 CRUD 方法，也不需要 XML 文件了
- 注意：实体类名应和数据库表名一致，否则会找不到！







# 三、MyBatis-Plus 核心功能

## 3.1 Mapper CRUD 接口

> 说明:
>
> - 通用 CRUD 封装 BaseMapper 接口，为 `Mybatis-Plus` 启动时自动解析实体表关系映射转换为 `Mybatis` 内部对象注入容器
> - 泛型 `T` 为任意实体对象
> - 参数 `Serializable` 为任意类型主键 `Mybatis-Plus` 不推荐使用复合主键约定每一张表都有自己的唯一 `id` 主键
> - 对象 `Wrapper` 为 条件构造器



### Insert

```java
// 插入一条记录
int insert(T entity);
```

| 类型 | 参数名 | 描述     |
| ---- | ------ | -------- |
| T    | entity | 实体对象 |



### Delete

```java
// 根据 entity 条件，删除记录
int delete(@Param(Constants.WRAPPER) Wrapper<T> wrapper);

// 删除（根据ID 批量删除）
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);

// 根据 ID 删除
int deleteById(Serializable id);

// 根据 columnMap 条件，删除记录
int deleteByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);
```

| 类型                               | 参数名    | 描述                                 |
| ---------------------------------- | --------- | ------------------------------------ |
| Wrapper<T>                         | wrapper   | 实体对象封装操作类（可以为 null）    |
| Collection<? extends Serializable> | idList    | 主键 ID 列表(不能为 null 以及 empty) |
| Serializable                       | id        | 主键 ID                              |
| Map<String, Object>                | columnMap | 表字段 map 对象                      |



### Update

```java
// 根据 whereWrapper 条件，更新记录
int update(@Param(Constants.ENTITY) T updateEntity, @Param(Constants.WRAPPER) Wrapper<T> whereWrapper);

// 根据 ID 修改
int updateById(@Param(Constants.ENTITY) T entity);
```

| 类型       | 参数名        | 描述                                                         |
| ---------- | ------------- | ------------------------------------------------------------ |
| T          | entity        | 实体对象 (set 条件值,可为 null)                              |
| Wrapper<T> | updateWrapper | 实体对象封装操作类（可以为 null,里面的 entity 用于生成 where 语句） |



### Select

```java
// 根据 ID 查询
T selectById(Serializable id);

// 根据 entity 条件，查询一条记录
T selectOne(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 查询（根据ID 批量查询）
List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);

// 根据 entity 条件，查询全部记录
List<T> selectList(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 查询（根据 columnMap 条件）
List<T> selectByMap(@Param(Constants.COLUMN_MAP) Map<String, Object> columnMap);

// 根据 Wrapper 条件，查询全部记录
List<Map<String, Object>> selectMaps(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 Wrapper 条件，查询全部记录。注意： 只返回第一个字段的值
List<Object> selectObjs(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 entity 条件，查询全部记录（并翻页）
IPage<T> selectPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 Wrapper 条件，查询全部记录（并翻页）
IPage<Map<String, Object>> selectMapsPage(IPage<T> page, @Param(Constants.WRAPPER) Wrapper<T> queryWrapper);

// 根据 Wrapper 条件，查询总记录数
Integer selectCount(@Param(Constants.WRAPPER) Wrapper<T> queryWrapper);
```

| 类型                               | 参数名       | 描述                                     |
| ---------------------------------- | ------------ | ---------------------------------------- |
| Serializable                       | id           | 主键 ID                                  |
| Wrapper<T>                         | queryWrapper | 实体对象封装操作类（可以为 null）        |
| Collection<? extends Serializable> | idList       | 主键 ID 列表(不能为 null 以及 empty)     |
| Map<String, Object>                | columnMap    | 表字段 map 对象                          |
| IPage<T>                           | page         | 分页查询条件（可以为 RowBounds.DEFAULT） |



## 3.2 Service CRUD 接口

> 说明:
>
> - 通用 Service CRUD 封装 IService 接口，进一步封装 CRUD 采用 `get 查询单行` `remove 删除` `list 查询集合` `page 分页` 前缀命名方式区分 `Mapper` 层避免混淆
> - 泛型 `T` 为任意实体对象
> - 建议如果存在自定义通用 Service 方法的可能，请创建自己的 `IBaseService` 继承 `Mybatis-Plus` 提供的基类
> - 对象 `Wrapper` 为 条件构造器

对比 Mapper 接口 CRUD 区别

- service添加了批量方法
- service层的方法自动添加事务



### 接口使用方式

```java
public interface UserService extends IService<User> {
}
```

```java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper,User> implements UserService{
}
```



### Save

```java
// 插入一条记录（选择字段，策略插入）
boolean save(T entity);

// 插入（批量）
boolean saveBatch(Collection<T> entityList);

// 插入（批量）
boolean saveBatch(Collection<T> entityList, int batchSize);
```

| 类型          | 参数名     | 描述         |
| ------------- | ---------- | ------------ |
| T             | entity     | 实体对象     |
| Collection<T> | entityList | 实体对象集合 |
| int           | batchSize  | 插入批次数量 |



### SaveOrUpdate

```java
// TableId 注解存在更新记录，否插入一条记录
boolean saveOrUpdate(T entity);

// 根据updateWrapper尝试更新，否继续执行saveOrUpdate(T)方法
boolean saveOrUpdate(T entity, Wrapper<T> updateWrapper);

// 批量修改插入
boolean saveOrUpdateBatch(Collection<T> entityList);

// 批量修改插入
boolean saveOrUpdateBatch(Collection<T> entityList, int batchSize);
```

| 类型          | 参数名        | 描述                             |
| ------------- | ------------- | -------------------------------- |
| T             | entity        | 实体对象                         |
| Wrapper<T>    | updateWrapper | 实体对象封装操作类 UpdateWrapper |
| Collection<T> | entityList    | 实体对象集合                     |
| int           | batchSize     | 插入批次数量                     |



### Remove

```java
// 根据 queryWrapper 设置的条件，删除记录
boolean remove(Wrapper<T> queryWrapper);

// 根据 ID 删除
boolean removeById(Serializable id);

// 根据 columnMap 条件，删除记录
boolean removeByMap(Map<String, Object> columnMap);

// 删除（根据ID 批量删除）
boolean removeByIds(Collection<? extends Serializable> idList);
```

| 类型                               | 参数名       | 描述                    |
| ---------------------------------- | ------------ | ----------------------- |
| Wrapper<T>                         | queryWrapper | 实体包装类 QueryWrapper |
| Serializable                       | id           | 主键 ID                 |
| Map<String, Object>                | columnMap    | 表字段 map 对象         |
| Collection<? extends Serializable> | idList       | 主键 ID 列表            |



### Update

```java
// 根据 UpdateWrapper 条件，更新记录 需要设置sqlset
boolean update(Wrapper<T> updateWrapper);

// 根据 whereWrapper 条件，更新记录
boolean update(T updateEntity, Wrapper<T> whereWrapper);

// 根据 ID 选择修改
boolean updateById(T entity);

// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList);

// 根据ID 批量更新
boolean updateBatchById(Collection<T> entityList, int batchSize);
```

| 类型          | 参数名        | 描述                             |
| ------------- | ------------- | -------------------------------- |
| Wrapper<T>    | updateWrapper | 实体对象封装操作类 UpdateWrapper |
| T             | entity        | 实体对象                         |
| Collection<T> | entityList    | 实体对象集合                     |
| int           | batchSize     | 更新批次数量                     |



### Get

```java
// 根据 ID 查询
T getById(Serializable id);

// 根据 Wrapper，查询一条记录。结果集，如果是多个会抛出异常，随机取一条加上限制条件 wrapper.last("LIMIT 1")
T getOne(Wrapper<T> queryWrapper);

// 根据 Wrapper，查询一条记录
T getOne(Wrapper<T> queryWrapper, boolean throwEx);

// 根据 Wrapper，查询一条记录
Map<String, Object> getMap(Wrapper<T> queryWrapper);

// 根据 Wrapper，查询一条记录
<V> V getObj(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```

| 类型                        | 参数名       | 描述                            |
| --------------------------- | ------------ | ------------------------------- |
| Serializable                | id           | 主键 ID                         |
| Wrapper<T>                  | queryWrapper | 实体对象封装操作类 QueryWrapper |
| boolean                     | throwEx      | 有多个 result 是否抛出异常      |
| T                           | entity       | 实体对象                        |
| Function<? super Object, V> | mapper       | 转换函数                        |



### List

```java
// 查询所有
List<T> list();

// 查询列表
List<T> list(Wrapper<T> queryWrapper);

// 查询（根据ID 批量查询）
Collection<T> listByIds(Collection<? extends Serializable> idList);

// 查询（根据 columnMap 条件）
Collection<T> listByMap(Map<String, Object> columnMap);

// 查询所有列表
List<Map<String, Object>> listMaps();

// 查询列表
List<Map<String, Object>> listMaps(Wrapper<T> queryWrapper);

// 查询全部记录
List<Object> listObjs();

// 查询全部记录
<V> List<V> listObjs(Function<? super Object, V> mapper);

// 根据 Wrapper 条件，查询全部记录
List<Object> listObjs(Wrapper<T> queryWrapper);

// 根据 Wrapper 条件，查询全部记录
<V> List<V> listObjs(Wrapper<T> queryWrapper, Function<? super Object, V> mapper);
```

| 类型                               | 参数名       | 描述                    |
| ---------------------------------- | ------------ | ----------------------- |
| Wrapper<T>                         | queryWrapper | 实体包装类 QueryWrapper |
| Collection<? extends Serializable> | idList       | 主键 ID 列表            |
| Map<String, Object>                | columnMap    | 表字段 map 对象         |
| Function<? super Object, V>        | mapper       | 转换函数                |



### Page

```java
// 无条件分页查询
IPage<T> page(IPage<T> page);

// 条件分页查询
IPage<T> page(IPage<T> page, Wrapper<T> queryWrapper);

// 无条件分页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page);

// 条件分页查询
IPage<Map<String, Object>> pageMaps(IPage<T> page, Wrapper<T> queryWrapper);
```

| 类型       | 参数名       | 描述                            |
| ---------- | ------------ | ------------------------------- |
| IPage<T>   | page         | 翻页对象                        |
| Wrapper<T> | queryWrapper | 实体对象封装操作类 QueryWrapper |



### Count

```java
// 查询总记录数
int count();

// 根据 Wrapper 条件，查询总记录数
int count(Wrapper<T> queryWrapper);
```

| 类型       | 参数名       | 描述                            |
| ---------- | ------------ | ------------------------------- |
| Wrapper<T> | queryWrapper | 实体对象封装操作类 QueryWrapper |



## 3.3 分页查询实现

第一步：启动类中导入分页插件

```java
@Bean
public MybatisPlusInterceptor mybatisPlusInterceptor() {
    // mybatis-plus 的插件集合
    MybatisPlusInterceptor mybatisPlusInterceptor = new MybatisPlusInterceptor();
    // 分页插件
    mybatisPlusInterceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
    return interceptor;
}
```

第二步：使用分页查询

```java
@SpringBootTest
public class MybatisPlusTest {
    @Autowired
    private StuMapper stuMapper;
    
    @Test
    public void testPage() {
        // IPage -> Page(页码, 页容量)
        Page<Student> page = new Page<>(1, 3);
        stuMapper.selectPage(page, null);
        
        long current = page.getCurrent(); // 页码
        long size = page.getSize(); // 页容量
        List<Student> records = page.getRecords(); // 当前页的数据
        long total = page.getTotal(); // 总条数
    }
}
```



## 3.4 条件构造器

> 使用 MyBatis-Plus 提供的条件构造器，可以灵活、高效地构建查询条件

### 3.4.1 条件构造器继承结构

Wrapper ： 条件构造抽象类，最顶端父类

- AbstractWrapper ： 用于查询条件封装，生成 sql 的 where 条件
    - QueryWrapper ： 查询/删除条件封装
    - UpdateWrapper ： 修改条件封装
    - AbstractLambdaWrapper ： 使用 Lambda 语法
        - LambdaQueryWrapper ：用于 Lambda 语法使用的查询 Wrapper
        - LambdaUpdateWrapper ： Lambda 更新封装 Wrapper

### 3.4.2 条件构造器的使用

示例代码：

```java
QueryWrapper<User> queryWrapper = new QueryWrapper<>();
queryWrapper.eq("name", "John"); // 添加等于条件
queryWrapper.ne("age", 30); // 添加不等于条件
queryWrapper.like("email", "@gmail.com"); // 添加模糊匹配条件
// 等同于： 
// delete from user where name = "John" and age != 30 and email like "%@gmail.com%"

// 链式调用
queryWrapper.eq("name", "John").ne("age", 30).like("email", "@gmail.com");

int i = xxxMapper.delete(queryWrapper);
```

### 3.4.3 条件构造器函数

[点击查看所有函数](https://baomidou.com/pages/10c804/#abstractwrapper)



## 3.5 核心注解

### 3.5.1 @TableName

- 描述：表名注解，标识实体类对应的表
- 位置：实体类

```java
@TableName("sys_user")
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

特殊情况：如果表名和实体类名相同（忽略大小写）可以省略该注解！

全局修改

```yaml
mybatis-plus: # mybatis-plus的配置
  global-config:
    db-config:
      table-prefix: sys_ # 表名前缀字符串
```



### 3.5.2 @TableId

- 描述：主键注解
- 位置：实体类主键字段

```java
@TableName("sys_user")
public class User {
    @TableId(value = "主键列名", type = IdType.XXX)
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

| 属性  | 类型   | 必须指定 | 默认值      | 描述         |
| ----- | ------ | -------- | ----------- | ------------ |
| value | String | 否       | ""          | 主键字段名   |
| type  | Enum   | 否       | IdType.NONE | 指定主键类型 |



IdType 属性常用值：

| 值                | 描述                                                         |
| ----------------- | ------------------------------------------------------------ |
| AUTO              | 数据库 ID 自增 (mysql配置主键自增长)                         |
| ASSIGN_ID（默认） | 分配 ID(主键类型为 Number(Long )或 String)(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextId`(默认实现类为`DefaultIdentifierGenerator`雪花算法) |

全局修改主键策略

```yaml
mybatis-plus:
  configuration:
    # 配置MyBatis日志
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      # 配置MyBatis-Plus操作表的默认前缀
      table-prefix: t_
      # 配置MyBatis-Plus的主键策略
      id-type: auto
```



### 3.5.3 雪花算法

雪花算法（Snowflake Algorithm）是一种用于生成唯一ID的算法。它由 Twitter 公司提出，广泛应用于分布式系统中，如微服务架构分布式数据库、分布式锁等场景，以满足全局唯一标识的需求

**雪花算法生成的数字，需要使用 Long 或者 String 类型主键**



### 3.5.4 @TableFiled

- 描述：字段注解（非主键）

```java
@TableName("sys_user")
public class User {
    @TableId
    private Long id;
    @TableField("nickname")
    private String name;
    private Integer age;
    private String email;
}
```

| 属性  | 类型    | 必须指定 | 默认值 | 描述               |
| ----- | ------- | -------- | ------ | ------------------ |
| value | String  | 否       | ""     | 数据库字段名       |
| exist | boolean | 否       | true   | 是否为数据库表字段 |







# 四、MyBatis-Plus 扩展

## 4.1 逻辑删除

使用逻辑删除，并不会删除表中的数据，而是更改数据的删除状态，便于后续的数据分析和恢复

逻辑删除的实现：

1. 数据库表中，添加逻辑删除字段，默认值为 0

2. 单一指定：实体类中，添加逻辑删除属性

   ```java
   @Data
   public class User {
   
      // @TableId
       private Integer id;
       private String name;
       private Integer age;
       private String email;
       
       @TableLogic
       //逻辑删除字段 mybatis-plus下，默认 逻辑删除值为1 未逻辑删除为0
       private Integer deleted;
   }
   ```

3. 全局指定：application.yaml

   ```yaml
   mybatis-plus:
     global-config:
       db-config:
         logic-delete-field: deleted
         logic-delete-value: 1 # 逻辑已删除值(默认为 1)
         logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
   ```



## 4.2 乐观锁

MyBatis-Plus 中提供了乐观锁插件，可以解决并发数据的问题

具体实现：

- 为每条数据添加一个版本号 version 字段
- 取出记录时，获取当前 version
- 更新时，检查 version 是否为最新
- 如果是最新 version，执行更新后，version += 1
- 如果不是最新 version，则更新失败

乐观锁插件使用：

1. 添加更新插件

   ```java
   @Bean
   public MybatisPlusInterceptor mybatisPlusInterceptor() {
       MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
       interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
       return interceptor;
   }
   ```

2. 乐观锁字段添加@Version 注解

   数据库表中也要添加 version 字段，默认值为 1

   - 支持的数据类型只有:int,Integer,long,Long,Date,Timestamp,LocalDateTime
   - 仅支持 `updateById(id)` 与 `update(entity, wrapper)` 方法

   ```java
   @Version
   private Integer version;
   ```



## 4.3 MyBatisX 快速开发

[官网查看](https://baomidou.com/pages/ba5b24/#%E5%8A%9F%E8%83%BD)
