# 数据库基本概念

DBMS ---> SQL ---> DB

- 数据库：简称 DB（DataBase）
  存储数据的仓库，实际上就是一堆文件，文件中存储特定格式的数据。



- 数据库管理系统：简称 DBMS（DataBaseManagementSystem）
  用来管理数据库中的数据，可以对数据进行增删改查（CRUD） 
  常见的数据库管理系统：MySQL、Oracle、MS SqlServer、DB2、sybase等等...



- SQL：结构化查询语言
  我们需要学习 SQL 语句，DBMS 会根据我们编写的 SQL 语句，对数据进行增删改查（CRUD）。



- 表：数据库中用表格表示数据，非常直观~
  - 行（row）：被称为数据 / 记录
  - 列（column）：被称为字段，字段有字段名、数据类型、约束等属性。







# SQL 服务

## 安装与卸载

[教程传送门](https://blog.csdn.net/pujun1201/article/details/119913745)



> port：3306
> 服务名：MySQL
> 用户名：root
> 密码：

## 服务启停

管理员身份运行命令行

```
启动：
	net start mysql
暂停：
	net stop mysql
```

## 本地登录 MySQL

命令行

```
显示密码：
	mysql -uroot -p密码
隐藏密码：
	mysql -uroot -p
```







# MySQL 常用命令

不区分大小写，以分号结尾

```sql
-- 查看所有数据库
	show databases;
	
-- 使用数据库
	use 数据库名;
	
-- 查看当前数据库
	select database();
	
-- 创建数据库
	create database 数据库名;
	
-- 查看当前库中有哪些表
	show tables;
	
-- 查看表结构
	desc 表名;
	
-- 查看建表语句
	show create table 表名;
	
-- 终止输入
	\c

-- 退出
	exit
```









# SQL 分类

结构：

- DDL (Data Definition Language)
  - create
  - drop
  - alter



数据：

- DQL (Data Query Language)
  - select
- DML (Data Manipulation Language)
  - insert
  - delete
  - update
- DCL (Data Control Language)
  - grant
  - revoke



事务

- TCL (Transaction Control Language)
  - commit
  - rollback







# DDL

## create 创建表

```sql
create table 表名(
    字段名1 数据类型, 
    字段名2 数据类型, 
    ..., 
    字段名n 数据类型 --> 这里没有逗号
);
```





## 复制表

将一个查询结果当成另一张表创建

```sql
create table 表 as (select * from 要复制的表);
```





## 常用数据类型

|   类型   |              描述               |
| :------: | :-----------------------------: |
| varchar  |        可变长度的字符串         |
|   char   |           定长字符串            |
|   int    |  整型（等同于 java 中的 int）   |
|  bigint  | 长整型（等同于 java 中的 long） |
|  float   |          单精度浮点型           |
|  double  |          双精度浮点型           |
|   date   |              日期               |
|   time   |              时间               |
| datetime |            日期时间             |

[更多参考](https://blog.csdn.net/u013120247/article/details/50915397?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166988286616800182736498%2522%252C%2522scm%2522%253A%252220140713.130102334.pc%255Fall.%2522%257D&request_id=166988286616800182736498&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~first_rank_ecpm_v1~hot_rank-17-50915397-null-null.142^v67^control,201^v3^add_ask,213^v2^t3_control1&utm_term=%E6%95%B0%E6%8D%AE%E5%BA%93%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B&spm=1018.2226.3001.4187)



## drop 删除表

语法

```sql
drop table 表名; --> 如果表不存在会报错

drop table if exists 表名;
```



## 修改表结构

实际开发中，很少修改，成本太高！修改表结构可以使用工具



> 了解（期末复习可看）
> 增：alter table 表名 add 字段名 数据类型 约束;
> 删：alter table 表名 drop 字段名;
> 修改数据类型：alter table 表名 modify 字段名 新数据类型;
> 修改表名：alter table 表名 rename to 新表名;









# DQL 单表查询

## 书写顺序和执行顺序

- 书写顺序

```sql
select
    ...
from
    ...
where
    ...
group by
    ...
having
    ...
order by
    ...
limit
    ...;
```

- 执行顺序
  1. from
  2. where
  3. group by
  4. having
  5. select
  6. order by
  7. limit





## select 简单查询

```sql
-- 单字段
	select 字段名 from 表名;
	
-- 多字段，用逗号隔开
	select 字段名1, 字段名2 from 表名;
	
-- 所有字段
	select * from 表名;
```

注意：

- 字段可以进行数学运算
- 实际开发中不建议 select * 这种写法，可读性差且效率低





## 别名

- as 可省略
- 当别名有空格或中文时，别名要加单引号

```sql
select 字段名 as 别名 from 表;

select 字段名 as '有空格或中文' from 表;
```





## 去重

```sql
select distinct 字段 from 表;
```





## where 条件查询

```sql
select
    字段1, 字段2, 字段3...
from
    表名
where
    条件;
```

|         条件          |              描述              |
| :-------------------: | :----------------------------: |
|    >、>=、=、<、<=    |          参考数学知识          |
|       <> 或 !=        |             不等于             |
|    between A and B    | 介于 A 和 B 之间，A 必须小于 B |
|          and          |              并且              |
|          or           |              或者              |
|          not          |    取非，主要用在 is 或 in     |
| is null / is not null |      为 null / 不为 null       |
|       in(a,b,c)       |   只要符合括号中任意一个即可   |
|         like          |            模糊查询            |

**and 的优先级比 or 高**





## 模糊条件查询

> 模糊查询有 百分号% 和 下划线_ 两种：
> 	% 匹配任意个（0也可以）字符
> 	_ 匹配一个字符

```sql
-- 例如：查询名字中含明的并且身份证号最后一位为 X 的学生
select 
    sname 
from 
    student 
where 
    sname like '%明%' and idnum like '%X'; -- 或 idnum like '_________________X' -> 这里是17个下划线，因为身份证有18位
```





## group by 分组查询

```sql
select
	...
from
	...
where
	...
group by 字段;
```





## having 分组条件过滤

> having 对分组后的数据进行过滤

```sql
select
	...
from
	...
where
	...
group by
	字段
having
	条件;
```

注意：

1. having 必须结合 group by 使用
2. 优先选择 where，where 完成不了用 having





## 分组函数（聚合函数）

> **分组函数使用时，必须先分组**



```sql
select 函数(字段) from 表;
```

|  函数   |   描述   |
| :-----: | :------: |
| count() | 统计数量 |
|  sum()  |   求和   |
|  avg()  |  平均值  |
|  max()  |  最大值  |
|  min()  |  最小值  |

注意：

1. 分组函数运算忽略null值，无需进行空处理
2. 分组函数不能直接在 where 子句中使用





## order by 排序

> order by 字段，根据字段对查询结果排序



- asc 升序（默认）
- desc 降序



```sql
-- 单字段排序
select
    字段
from
    表名
order by
    字段 desc; -->根据字段降序

-- 多字段排序
select
    字段
from
    表名
order by
    字段1 asc, 字段2 desc; --> 先按照字段1排序，当字段1相同时，再排字段2
```





## limit 分页查询

> limit 约束查询结果的行数，即查询某个区间的数据



1. limit length;

   ```sql
   select
   	...
   from
   	...
   limit 5; -- 查询前5条数据
   ```

   

2. limit startIndex, length

   ```sql
   select
   	...
   from
   	...
   limit 2, 5 -- 查询第3-5条数据
   ```

- startIndex = (pageNo - 1) * length

- limit (pageNo - 1) * pageSize, pageSize







# DQL 多表查询

## 连接查询

语法
`select 字段1, 字段2 from 表1, 表2;`



分类：

- 内连接
- 外连接
- 全连接



### 笛卡尔积现象

当两张表进行连接查询，在没有任何条件限制时，最终查询结果数 = 两张表行数乘积



### 消除笛卡尔积

```sql
-- SQL92语法
select
    表1.字段, 表2.字段
from
    表1, 表2
where
    表1.字段 = 表2.字段;
```





## 连接查询 - 内连接

> 内连接的表之间没有主次关系

### 等值连接

连接条件是等值关系

```sql
-- SQL99语法（推荐）- 表连接条件独立
select
    表1.字段, 表2.字段
from
    表1
inner join -- inner可省略
    表2
on
    表1.字段 = 表2.字段 -- 等值
where
    ...;
```

### 非等值连接

连接条件不是等量关系

```sql
select
    e.ename, e.sal, s.grade
from
    emp e
inner join
    salgrade s
on
    e.sal between s.losal and s.hisal; -- 不是等量关系
```

### 自连接

把一张表看成两张表

```sql
select
    a.ename as '员工名', b.ename as '领导名'
from
    emp a
inner join
    emp b
on
    a.mgr = b.empno; -- 员工的领导编号 = 领导的员工编号
```







## 连接查询 - 外连接

> 外连接的表之间有主次关系
>
> 
>
> 主表的数据会被全部查询出来（左或右出现哪个，哪个就是主表）

### 右外连接（右连接）

```sql
select
    e.name, d.name
from
    dept d
right outer join -- 出现 right，右边的 emp 为主表
    emp e
on
    e.deptno = d.deptno;
```

### 左外连接（左连接）

```sql
select
    e.name, d.name
from
    dept d
left outer join -- 出现 left，左边的 dept 为主表
    emp e
on
    e.deptno = d.deptno;
```







## 多表连接

```sql
-- 内外连接可以混合
select
    ...
from
    a
join
    b
on
    a和b的连接条件
join
    c
on
    a和c的连接条件
...
```







## 子查询

select 语句中嵌套 select 语句
可以出现在 select、from、where 子句中

- where

```sql
-- 案例：找出比最低工资高的员工姓名和工资
select
    ename, sal
from
    emp
where
    sal > (select min(sal) from emp);
```

- from
  技巧：将子查询的结果当作一张临时表

```sql
-- 案例：找出每个岗位的平均工资的薪资等级
select
    e.*, s.grade
from
    (select job, avg(sal) as avgsal from emp group by job) as e
inner join
    salgrade as s
on
    avgsal between s.losal and s.hisal;
```

- select（了解）
  select 后边的子查询只能返回1条结果，否则报错

```sql
-- 案例：找出每个员工的部门名称，要求显示员工名和部门名
select
    e.name,
    (select d.dname from dept d where e.deptno = d.deptno) as dname
from
    emp e;
```





## union

> 表连接时，匹配的次数满足笛卡尔积（匹配次数 = 两条语句次数乘积）
>
> 
>
> 使用 union 在减少匹配次数时，还可以对查询结果进行拼接，效率更高（匹配次数 = 两条语句次数之和）
>
> 
>
> 注意：字段的个数应相同

```sql
select 字段 from 表
union
select 字段 from 表;
```







# DML

## insert 插入数据

```sql
-- 一条数据
insert into 表(字段1, 字段2...) values
    (值1, 值2...);

-- 多条数据
insert into 表(字段1, 字段2...) values
    (值1, 值2...),
    (值1, 值2...),
    ...;
```

注意：

- 表后字段省略，相当于所有字段
- 字段与值的个数和类型要对应
- 其他字段默认为 null





## delete 删除数据

```sql
delete from 表 where 条件; -- 没有条件限制会删除所有数据
```

优点：支持回滚，可以恢复数据

缺点：效率低





## truncate 删除数据(DDL)

```sql
truncate table 表;
```

删除数据后表还在

优点：效率高

缺点：不持支回滚





## update 修改数据

```sql
update 表名 set 字段1=值1, 字段2=值2... where 条件; -- 没有条件限制会更新所有数据
```







# 约束

> 约束（constraint），创建表时对字段进行约束，保证数据的有效性和完整性

- 非空约束：not null

- 唯一性约束：

  - 列唯一：unique 

  - 联合唯一约束：unique(字段1，字段2...)

- 主键约束：primary key 主键非空且唯一

  - 单一主键
  - 复合主键

- 外键约束：foreign key

- 默认值约束：default

- 检查约束：check（mysql不支持，oracle支持）



## 外键约束

> 添加外键，可以连接两张表，减少数据的冗余

- 外键可以为 null
- 与外键关联的字段必须为 unique



语法：

`foreign key (外键名) references 另一张表名(另一张表的字段)`





例：学生表添加外键，关联课程表

```sql
create table student{
    sno char(8) primary key,
    ssex char(1) not null,
    foreign key (s_cno) references course(cno)
};
```







# 存储引擎

> 存储引擎是 MySQL 中特有的术语，不同的存储引擎，表存储数据的方式不同

## 指定存储引擎

可以在建表时指定存储引擎



MySQL默认存储引擎：InnoDB	默认字符编码方式：utf8

```sql
create table name(
	...
)ENGINE = InnoDB DEFAULT CHARSET = utf8;
```

## 查看支持哪些引擎

`show engines \G`

## 常用存储引擎

- InnoDB
- MyISAM
- MEMORY

### InnoDB

默认，重量级引擎

主要特点：安全，支持事务



表的内容存储在 InnoDB 表空间 tablespace（存储数据+索引）

### MyISAM

特点：可被转换为压缩、只读表来节省空间



每个表对应三个文件：

- 格式文件（frm）
- 数据文件（MYD）
- 索引文件（MYI）

### MEMORY

数据和索引都存储在内存中

优点：查询效率最高，不需要和硬盘交互

缺点：不安全，关机后数据消失







# TCL 事务

> 一个事务（transaction）就是一个完整的业务逻辑，是最小的工作单元，不可再分
>
> 例如：
>
> ​	假设转账，从 A 向 B
>
> ​	A 的钱减去 100
>
> ​	B 的钱增加 100
>
> 以上就是一个完整的业务逻辑

只有 DML 语句和事务有关

- insert
- delete
- update



## 提交和回滚

InnoDB存储引擎：提供一组记录事务性活动的日志文件

1. 每一条DML操作都会记录到日志文件中，提交或回滚对日志文件进行处理

2. 默认自动提交（每执行一条DML语句，提交一次）

   可以使用 `start transaction;` 开启事务，就可以关闭自动提交

3. 回滚只能回滚到上一次的提交点



- 提交：commit

  ```sql
  start transaction; -- 开启事务
  
  ...; -- DML语句
  
  commit;
  ```

  

- 回滚：rollback

  ```sql
  start transaction;
  
  ...; -- DML语句
  
  rollback;
  ```





## 事务的特性

A（Atomicity）、C（Consistency）、I（Isolation）、D（Durability）

- A：原子性

  事务是最小的工作单元，不可再分

- C：一致性

  在同一事务中，所有操作必须同时成功或失败，保证数据的一致性

- I：隔离性

  事务 A 与事务 B 之间具有一定的隔离

- D：持久性

  事务结束的保障。事务提交，相当于把没有保存到硬盘上的数据保存到硬盘上





## 隔离级别

> 大多数数据库隔离级别都是二档起步
>
> MySQL 的默认隔离级别是 repeatable read

- 读未提交：read uncommitted

  ​	事务 A 可以读取到事务 B 未提交的数据

  ​	存在问题：脏读现象，读到脏数据

  

- 读已提交：read committed

  ​	事务 A 只能读取到事务 B 提交后的数据

  ​	解决了脏读，但是不可重复读取数据

  

- 可重复读：repeatable read

  ​	事务开启后，每次读取当前事务的数据都是一致的，直到该事务结束

  ​	存在问题：幻影读，读到的数据都是幻象

  

- 序列化 / 串行化：serializable

  ​	隔离级别最高，解决了所有问题，但效率最低，事务排队，不能并发

  

```sql
-- 设置全局隔离级别
  set global transaction isolation level 隔离级别;
  
-- 查看隔离级别
  select @@transaction_isolation; -- mysql 8.0
```







# 索引

> 索引添加在字段上，可以提高查询效率，相当于书的目录
>
> 如果字段不加索引，会进行全扫描
>
> MySQL 查询两种方式：
>
> ​	一：全表扫描
>
> ​	二：根据索引检索
>
> 索引在 MySQL 中都是以 B-Tree 形式存在



注意：

1. 主键 和 unique 约束的字段都会自动创建索引
2. 存储引擎不同，索引存储方式不同
   - InnoDB：tablespace
   - MyISAM：.MYI
   - MEMORY：内存
3. 不要随意添加索引，太多会降低系统的性能



## 何时添加索引

1. 数据量庞大
2. 字段经常出现在 where 后边，也就是该字段总是被扫描
3. 字段很少进行 DML 操作（DML 操作后，索引会重新排序）



## 创建和删除

```sql
-- 创建
create index 索引名 on 表名(字段);

-- 删除
drop index 索引名 on 表;
```



## 查看有无索引

```sql
explain select * from 表 where 要查看的字段;
```



## 索引失效

1. 模糊匹配时 % 开头
2. 使用 or 时，or 的一侧没有索引
3. 使用复合索引（多个字段联合起来添加索引）时，没有使用左边的字段查找
4. 在 where 中索引字段参与了运算
5. 在 where 中索引字段使用了函数



## 索引分类

- 单一索引
- 复合索引
- 主键索引
- 唯一性索引







# 视图

> 视图（view）是一张虚拟表，可以将复杂的 SQL 语句创建为一个视图，简化开发，利于维护



## 创建和删除视图

```sql
-- 创建
create view 视图名 as DQL语句;

-- 删除
drop view 视图名;
```



## 视图的作用

使用视图可以将复杂的 SQL 语句创建成视图对象，每次使用该语句时，可以直接用视图，简化开发



之前我们已经学过表的复制，但是操作新表数据，原表数据不会被操作

```sql
create table 表 as (select * from 要复制的表);
```



**但是对视图的操作，会影响原表数据**

```sql
select * from 视图名;

insert into 视图名(字段) values (...);

select * from 原表; -- 可以发现原表数据已修改
```







# DCL

## 用户管理

```sql
-- 创建用户
create user '用户名'@'主机名' identified by '密码';

-- 删除用户
drop user '用户名'@'主机名';

-- 查看用户信息
select * from mysql.user;
```



## 权限管理

```sql
-- 查看用户权限
show grants for '用户名'@'主机名';

-- 授权
grant 权限 on 数据库名.表名 to '用户名'@'主机名';

-- 撤销授权
revoke 权限 on 数据库名.表名 from '用户名'@'主机名';
```



## 用户权限

|        权限名         |         说明         |
| :-------------------: | :------------------: |
| all 或 all privileges |       所有权限       |
|        create         |    创建数据库或表    |
|         drop          | 删除数据库、表或视图 |
|         alter         |        修改表        |
|        select         |       查询数据       |
|        insert         |       插入数据       |
|        update         |       修改数据       |
|        delete         |       删除数据       |



## 数据导出

导出数据为 dos 命令，非 mysql 命令

```
导出数据库：
  mysqldump 数据库名>路径 -u用户名 -p密码

导出数据库中的表：
  mysqldump 数据库名 表名>路径 -u用户名 -p密码
```



## 数据导入

```sql
source 路径
```







# 数据库设计三范式

> - 第一范式：要求任何一张表必须有主键，每一个字段原子性不可再分
>
> - 第二范式：建立在第一范式的基础上，要求所有非主键字段必须依赖主键，不要产生部分依赖
>
> - 第三范式：建立在第二范式的基础上，要求所有非主键字段必须直接依赖主键，不要产生传递依赖
>
> - 数据库三范式是理论上的
>
>   最终的目的都是为了满足客户的需求，有时会拿冗余换执行速度
>
>   因为在SQL语句中，表和表之间连接越多，效率越低（笛卡尔积）

## 第一范式

最核心，最重要的范式，所有表的设计都要满足第一范式。

必须有主键，每个字段都是原子性，不可再分



例如：学生信息表

| 学生编号 | 学生姓名 | 联系方式                  |
| -------- | -------- | ------------------------- |
| 1001     | 张三     | zs@email.com, 13599999999 |
| 1002     | 李四     | ls@email.com, 13588888888 |
| 1001     | 王五     | ww@email.com, 13577777777 |

以上表的设计不满足第一范式，因为没有主键，且联系方式字段不具有原子性（联系方式可以拆分为邮箱和联系电话）

| 学生编号（pk） | 学生姓名 | 邮箱地址     | 联系电话    |
| -------------- | -------- | ------------ | ----------- |
| 1001           | 张三     | zs@email.com | 13599999999 |
| 1002           | 李四     | ls@email.com | 13588888888 |
| 1003           | 王五     | ww@email.com | 13577777777 |

修改之后，满足第一范式







## 第二范式

建立在第一范式的基础上，要求所有非主键字段必须依赖主键，不要产生部分依赖



例如：学生和教师表

学生和老师的关系：多对多（一个学生可以有多个老师，一个老师可以有多个学生）

| 学生编号 | 学生姓名 | 教师编号 | 教师姓名 |
| -------- | -------- | -------- | -------- |
| 1001     | 张三     | 001      | 王老师   |
| 1002     | 李四     | 002      | 赵老师   |
| 1003     | 王五     | 001      | 王老师   |
| 1001     | 张三     | 002      | 赵老师   |

以上表的设计不满足第一范式

| 学生编号 + 教师编号（pk） | 学生姓名 | 教师姓名 |
| ------------------------- | -------- | -------- |
| 1001              001     | 张三     | 王老师   |
| 1002              002     | 李四     | 赵老师   |
| 1003              001     | 王五     | 王老师   |
| 1001              002     | 张三     | 赵老师   |

学生编号和教师编号联合做主键，复合主键（PK：学生编号 + 教师编号）

上表满足第一范式，但不满足第二范式，产生了部分依赖

部分依赖：“张三”依赖 1001，“王老师”依赖 001，即“张三”和“王老师”重复了，数据冗余



如下设计满足第二范式：

​	学生表

| 学生编号（pk） | 学生姓名 |
| -------------- | -------- |
| 1001           | 张三     |
| 1002           | 李四     |
| 1003           | 王五     |



​	教师表

| 教师编号（pk） | 教师姓名 |
| -------------- | -------- |
| 001            | 王老师   |
| 002            | 赵老师   |



​	学生教师关系表

| id（pk） | 学生编号（fk） | 教师编号（fk） |
| -------- | -------------- | -------------- |
| 1        | 1001           | 001            |
| 2        | 1002           | 002            |
| 3        | 1003           | 001            |
| 4        | 1001           | 002            |







## 第三范式

第三范式：建立在第二范式的基础上，要求所有非主键字段必须直接依赖主键，不要产生传递依赖



例如：学生和班级表

班级和学生的关系：一对多（一个班可以有多个学生）

| 学生编号（pk） | 学生姓名 | 班级编号 | 班级名称 |
| -------------- | -------- | -------- | -------- |
| 1001           | 张三     | 01       | 一年一班 |
| 1002           | 李四     | 02       | 一年二班 |
| 1003           | 王五     | 03       | 一年三班 |
| 1004           | 赵六     | 03       | 一年三班 |

上表满足第一范式（主键 + 原子性），满足第二范式（单一主键，没有产生部分依赖）

不符合第三范式，产生了传递依赖：一年一班 依赖 01，01 依赖 1001



如下设计满足第三范式：

​	班级表（一）

| 班级编号（pk） | 班级名称 |
| -------------- | -------- |
| 01             | 一年一班 |
| 02             | 一年二班 |
| 03             | 一年三班 |



学生表（多）

| 学生编号（pk） | 学生姓名 | 班级编号（fk） |
| -------------- | -------- | -------------- |
| 1001           | 张三     | 01             |
| 1002           | 李四     | 02             |
| 1003           | 王五     | 03             |
| 1004           | 赵六     | 03             |





## 设计总结

根据字段间的关系：

- 一对一

  一对一，外键唯一（实际开发中，字段太多的话需要拆分）

- 一对多

  一对多，两张表，多的表加外键

- 多对多

  多对多，三张表，关系表俩外键





一对多和多对多参考上边的例子



例，一对一：用户表

未拆分：

  t_user

| id   | login_name | login_pwd | real_name | email...     |
| ---- | ---------- | --------- | --------- | ------------ |
| 1    | zs         | 123456    | 张三      | zs@email.com |
| 2    | ls         | 123123    | 李四      | ls@email.com |



拆分后：

  t_login 登陆信息表

| id（pk） | login_name | login_pwd |
| -------- | ---------- | --------- |
| 1        | zs         | 123456    |
| 2        | ls         | 123123    |



  t_user 用户详细信息表

| id(pk) | real_name | email...     | login_id(fk + unique) |
| ------ | --------- | ------------ | --------------------- |
| 100    | 张三      | zs@email.com | 1                     |
| 200    | 李四      | ls@email.com | 2                     |
