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







# 事务
