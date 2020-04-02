---
title: 学习mysql
date: 2019-07-24 18:12:19
tags: mysql
categories: 后端
---

## 基础
1. 创建数据库
```js
CREATE DATABASE [数据库名]
```
2. 删除数据库
```js
DROP DATABASE [数据库名]
```
3. 选择数据库
```js
USE [数据库名]
```
4. 创建数据库表
```js
CREATE TABLE table_name (column_name column_type)CREATE TABLE IF NOT EXISTS `runoob_tbl`(   `id` INT UNSIGNED AUTO_INCREMENT,   `title` VARCHAR(100) NOT NULL,   `author` VARCHAR(40) NOT NULL,   `date` DATE,   PRIMARY KEY ( `id` ))ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
5. 修改数据库表 alter
```js
// 添加
ALTER TABLE student ADD  

```
5. 删除数据库表
```js
DROP TABLE table_name
```
6. 查询数据数量
```js
SELECT count(*) FROM db_test.t_student WHERE gender="M";
```
7. 查询最小值的 min
```js
SELECT min(birthdate) FROM db_test.t_student;
```
8. 查询最大值的 max
```js
SELECT max(birthdate) FROM db_test.t_student;
```
9. 求和  sum
```js
SELECT SUM(OrderPrice) AS OrderTotal FROM Orders;
```
10. 条件查询
```js
SELECT * FROM t_student WHERE birthdate >='1991-01-01' AND birthdate <= '1993-12-31'; //两天语句效果一样
SELECT * FROM t_student WHERE birthdate BETWEEN '1991-01-01' AND '1993-12-31';
```

10. 模糊查询,使用LIKE没有通配符的时候和=效果相通,%是通配符相当任意字符
```js
// 查找name字段‘王’开头的数据
SELECT * FROM t_student WHERE name LIKE '王%'; 
```
11. 排序(默认顺序ASC,DESC倒序排序)
```js
// 默认排序
SELECT * FROM t_student ORDER BY birthdate; 
//倒序排序
SELECT * FROM t_student ORDER BY birthdate DESC; 
```
12. 多表查询(LEFT JOIN)
```js
// 查询的数据显示所有字段
SELECT * FROM t_student,t_class WHERE t_student.class_id = t_class.class_id; 

//查询的字段 指定显示 id、name、class_name
SELECT t_student.id,t_student.name,t_class.class_name FROM t_student,t_class WHERE t_student.class_id = t_class.class_id;  

//left join 和上面where效果一样
SELECT t_student.id,t_student.name,t_class.class_name FROM t_student LEFT JOIN t_class ON t_student.class_id = t_class.class_id; 
```
