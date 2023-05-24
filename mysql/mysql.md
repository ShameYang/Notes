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
启动：net start mysql
暂停：net stop mysql
```

## 本地登录 MySQL

命令行

```
显示密码：mysql -uroot -p密码
隐藏密码：mysql -uroot -p
```







# MySQL 常用命令

不区分大小写，以分号结尾

```mysql
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