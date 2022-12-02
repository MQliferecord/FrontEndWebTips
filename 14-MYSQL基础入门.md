# MYSQL基础入门

以下内容都是B站 **@遇见狂神说** 的MYSQL课程的笔记。

## 1.初始MySQL

### 1.1数据库分类：

- 关系型数据库：

MySQL/Oracle/Sql Server/DB2等等，主要是通过表和表，行和列之间进行数据的存储。

- 非关系型数据库：

Redis/MongDB，对象存储，通过对象自身的属性。

### 1.2数据库基础使用命令

- 打开MySQL: `mysql -u root -p密码;`

- 修改密码权限: `update mysql.user set authentication_string=password('${实际密码}') where user='root' and Host = 'localhost';`

- 刷新权限：`flush privileges;`

- 查看全部数据库：`show databases;`

- 使用某个数据库：`use ${数据库名};`

- 查看其中的表：`show tables;`

- 显示数据库中表信息：`describe ${表名};`

- 创建数据库：`create database ${数据库名};`

- 推出连接：`exit;`

- 单行注释：`--`

- 多行注释：`/** */`

### 1.3数据库语言

- DDL 定义

- DML 操作

- DQL 查询

- DCL 控制

## 2.操作数据库

操作数据库>操作数据库中的表>操作表中的数据

### 2.1操作数据库

- 创建数据库

`create database [if not exists] ${数据库名}`

- 删除数据库

`drop database [if exists] ${数据库名}`

- 使用数据库

切换数据库：`use ${数据库名}`

- 查看数据库

查看所有的数据库：`show databases`

### 2.2数据库的数据类型

> 数值

- tinyint 一个字节 

- smallint 两个字节

- **int 四个字节（标准整数）**

- bigint 八个字节

- float 单精度浮点数 四个字节

- double 双精度浮点数 八个字节

- **decimal 字符串形式的浮点数 （金融计算）**

> 字符串

- char 0-255

- **varchar 可变长字符串 0-65535** 对应Java中的String

- tinytext 微型文本 2^8-1

- **text 文本串 2^16-1**

> 时间日期

- date YYYY-MMMM-DDDD 日期格式

- time HH:MM:SS 时间格式

- **datetime YYYY-MMMM-DDDD HH:MM:SS最常用时间格式**

- **timestamp 时间戳，从1970.1.1到现在的毫秒数**

- year 年份

> null

没有值，空的，注意不要使用NULL进行运算，因为无论是什么运算都会得到结果NULL

### 2.3数据库的字段属性

- 主键 id

- Unsigned 无符号整数，不能声明为负数

- zerofill 零填充，不足的位数使用零来填充

- 自增 自动在上一条记录的基础上+1,通常来设置为唯一的主键，并且必须是整数类型

- 非空 NULL和not NULL如果设置为not null则不给他赋值就会报错；NULL则默认NULL

- 默认 不赋值前默认划分的值

每个表都必须存在以下的字段

```MySQL
/**
id 主键

`version` 乐观锁

is_delete 伪删除

gmt_create 创建时间

gmt_update 修改时间
*/

```
### 2.4数据库表

> 实战案例演示：

```MySQL
-- 目标：创建一个school数据库
-- 创建学生表（列，字段）使用SQL创建
-- 学号int 密码varchar(20) 姓名、性别varchar(2) 出生日期（datatime）家庭住址，email

-- AUTO_INCREMENT 自增
-- PRIMARY KEY 一般都是id

CREATE TABLE IF NOT EXISTS `student` (
    `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名'，
    `password` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码'，
    `sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别'，
    `birthday` DATETIME DEFAULT NULL COMMENT '出生日期'，
    `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址'，
    `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱'，
    PRIMARY KEY(`id`)
)ENGINE=INNODB DEFAULT CHARSET=utf8
```

格式

```mysql
CREATE TABLE [IF NOT EXISTS] `表名`(
    '字段名' 列类型 [属性][索引][注释],
    '字段名' 列类型 [属性][索引][注释],
    ......
    '字段名' 列类型 [属性][索引][注释]
)[表类型][字符集设置][注释]
```

- 查看创建数据库的语句：`SHOW CREATE DATABASE school`

- 查看创建表的定义语句：`SHOW CREATE TABLE student`

- 显示表结构：`DESC student`

### 2.5数据表的引擎

- innoDB 默认使用

1. 事务支持：支持||数据行锁定：支持||外键约束：支持||全文索引：不支持||表空间大小：较大

2. 安全性高，事务处理，多表多用户操作

- Myisam 早先年使用

1. 事务支持：不支持||数据行锁定：不支持||外键约束：不支持||全文索引：支持||表空间大小：较小

2. 节约空间，速度较快

> 所有的数据库都存在data下，所有的数据库其实对应的本质是一个文件存储。

数据库引擎在物理文件上的区别：

1. innoDB在数据库中只有 *.frm 文件，以及上级目录的ibdata的文件

2. myisam文件则产生三个文件：*.frm 表结构的定义文件、*.MYD 数据文件、*.MYI 索引文件

### 2.6 修改删除表

> 修改

修改表名：`ALTER TABLE ${旧表名} RENAME AS ${新表名}`--ALTER TABLE teacher RENAME AS teach

增加表的字段：`ALTER TABLE ${表名} ADD ${属性名 属性性质} `--ALTER TABLE teach ADD age INT(11)

修改表的字段性质：`ALTER TABLE ${表名} MODIFY ${属性名 属性性质}`--ALTER TABLE teach MODIFY age VARCHAR(11)

修改表的字段名：`ALTER TABLE ${表名} CHANGE ${属性名 属性性质}`--ALTER TABLE teach CHANGE age age1 VARCHAR(11)

> 删除

删除表的字段：`ALTER TABLE ${表名} DROP ${属性名}`--ALTER TABLE teach DROP age1

删除表：`DROP TABLE IF EXISTS teach`--DROP TABLE IF EXISTS teach

**创建和删除操作尽量加上约束条件**

## 3.MySQL数据管理

### 3.1外键（了解）

假设还有一个表格：

学生表的gradeid字段要去引用年级表的gradeid，定义外键key，给这个外键添加约束（执行引用）

- 方式一：在创建表的时候增加约束

```mysql
CREATE TABLE IF NOT EXISTS `grade` (
    `gradeid` INT(10) NOT NULL AUTO_INCREMENT COMMENT '年级id',
    `gradename` VARCHAR(50) NOT NULL COMMENT '年级名称'，
    PRIMARY KEY(`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

CREATE TABLE IF NOT EXISTS `student` (
    `id` INT(4) NOT NULL AUTO_INCREMENT COMMENT '学号',
    `name` VARCHAR(30) NOT NULL DEFAULT '匿名' COMMENT '姓名'，
    `password` VARCHAR(20) NOT NULL DEFAULT '123456' COMMENT '密码'，
    `sex` VARCHAR(2) NOT NULL DEFAULT '男' COMMENT '性别'，
    `birthday` DATETIME DEFAULT NULL COMMENT '出生日期'，
    `gradeid` INT(10) NOT NULL COMMENT '学生的年级'，
    `address` VARCHAR(100) DEFAULT NULL COMMENT '家庭住址'，
    `email` VARCHAR(50) DEFAULT NULL COMMENT '邮箱'，
    PRIMARY KEY(`id`)
    KEY `FK_gradeid` (`gradeid`)
    CONSTRAINT `FK_gradeid` FOREIGN KEY ('gradeid') REFERENCES `grade` (`gradeid`)
)ENGINE=INNODB DEFAULT CHARSET=utf8

```

- 方式二：在外部方法添加约束：

`ALTER TABLE ``student`` ADD CONSTRAINT 'FK_gradeid' FOREIGN KEY ('gradeid') REFERENCES ``grade`` (``gradeid``)`

**!以上都是物理外键，数据库级别的外键，不建议使用！因为如果要删除一张表可能会联系到其他的表，导致无法删除。**

最佳实践：

- 数据库就是单纯的表，只用来存数据。

- 使用程序去实现外键的功能。

### 3.2DML语言（全部记住）数据库管理语言

DML语言：数据操作语言

- Insert 

- update

- delete

### 3.3添加

`INSERT INTO 表名(字段名1，字段名2...) value(值1，值2)`

实际的语句的例子：

```mysql
INSERT INTO `grade`(`gradename`) VALUES(大四')

INSERT INTO `grade`(`gradeid`,`gradename`) VALUES('1','大三')

INSERT INTO `grade`(`gradename`) 
VALUES('大二'),('大一')
```

### 3.4修改

`UPDATE ${表名} SET ${属性}='属性名' WHERE [条件]`;

```mysql
UPDATE `student` SET `name`='狂神' WHERE id=1;

UPDATE `student` SET `name`='长江七号',`email`='123@123.com' WHERE id=2
```

条件(where 字句运算符 操作返回boolean):

条件如果没有指定则会修改所有的值

- `=` 等于 eq:5=6 false

- `<>`或者`！= `不等于 eq:5<>5 true

- `>` `>=` 大于

- `<` `<=` 小于

- `BETWEEN {} AND {}` 在某个区间

- `AND` 与

- `OR` 或


### 3.5删除

`DELETE FROM ${表名} WHERE [条件]`

案例：

```mysql
DELETE FROM `student` WHERE id=1
```
### 3.6清空

`TRUNCATE ${表名}`

案例：

```mysql
TRUNCATE `student`
```

> delete和truncate 的区别

- 相同点：都能够删除数据，不会删除表结构

- 不同点：truncate会重新设计自增列，计数器会归零，并且不会影响事务。delete却会继承自增的序列，比如已经有1-3行数据，然后删除1-3列数据，再增加数据truncate会id回到1，delete会继续从4开始

## 4.DQLy查询数据（最重点）

### 4.1DQL(Data Query Language:数据查询语言)

- 所有查询操作

- 数据库中最核心语言

### 4.2指定查询字段\

> 基础查询SELECT

`SELECT ${字段} FROM ${表格}`

```mysql
-- 查询所有的学生
SELECT * FROM student

-- 查询指定的字段
SELECT `studentNo`,`studentName` FROM `student`

--别名，给结果起一个名字 AS 可以给字段和表起别名
SELECT `studentNo` AS 学号，`studentName` AS 学生姓名 FROM student

-- 函数，CONCAT(A,B)
SELECT CONCAT('姓名：'，studentName) AS 新名字 FROM student
```

> 去重 DISTINCT

去除重复的数据，只显示一条

```mysql
-- 查询有哪些同学参加了考试
SELECT * FROM result

--查询有哪些同学参加考试
SELECT `studentno` FROM result

-- 去重
SELECT DISTINCT `studentno` FROM result

--查看系统版本
SELECT VERSION()

SELECT 100*3-1 AS 计算结果

SELECT @@auto_increment_increment

SELECT `studentno`,`studentresult`+1 AS '提分后' FROM result
```

数据库中的表达式：文本值，列，null， 计算表达式，系统变量...

### 4.3WHERE条件字句

作用：检索其中符合条件的值

> 逻辑运算符

- 与： AND &&

- 或： OR ||

- 非：NOT ！

```mysql
SELECT `studentno`, `studentname` FROM result 
WHERE studentresult>=95 AND studentresult<=100

SELECT `studentno`, `studentname` FROM result 
WHERE studentresult>=95 && studentresult<=100

SELECT `studentno`, `studentname` FROM result 
WHERE studentresult BETWEEN 95 AND 100

SELECT `studentno`, `studentname` FROM result 
WHERE NOT studentno=1000

SELECT `studentno`, `studentname` FROM result 
WHERE studentno!=1000
```

> 模糊查询

- 空：IS NULL

- 非空：IS NOT NULL

- 区间：BETWEEN ... AND ...

- 匹配：LIKE +  %(代表省略任意的字符个数)  _(代表省略一个字符)

- 范围：IN

```mysql
--查询姓刘的同学
SELECT  `studentno`,`studentname` FROM `student`
WHERE studentname LIKE '刘%';

--查询刘后跟一个字符串
SELECT  `studentno`,`studentname` FROM `student`
WHERE studentname LIKE '刘_';

--查询刘后跟两个字符串
SELECT  `studentno`,`studentname` FROM `student`
WHERE studentname LIKE '刘__';

--查询包含嘉字
SELECT  `studentno`,`studentname` FROM `student`
WHERE studentname LIKE '%嘉%';

SELECT  `studentno`,`studentname` FROM `student`
WHERE studentno IN (1001,1002,1003);

SELECT  `studentno`,`studentname` FROM `student`
WHERE `address` IN ('安徽','北京');
```

### 4.4联表查询

> JOIN 对比

实际的JOIN的方法有三种：

首先先确定这两个表一定有一个字段存在两张表中，比如案例中的studentno，但是两张表中的studnetno的数值是不一定相同的。

- INNER JOIN

返回两张表中重合的那部分数据。

- LEFT JOIN

即使student表中的studentno没有相关的信息，但是只要左表result中studentno有相关信息依然返回。

- RIGHT JOIN

即使result表中的studentno没有相关的信息，但是只要右表student中studentno有相关信息依然返回。

思路：

1. 分析需求，分析查询的字段来自于那些表

2. 确定使用那些查询方式，确定两张表中的交叉点（两个表中哪些部分是交集）

比如：学生表studentNo === 成绩表studentNo

```mysql
SELECT * FROM student
SELECT * FROM result

-- INNER JOIN
SELECT s.studentno,studentname,subjectno,studentresult
FROM student AS s
INNER JOIN result AS r
ON s.studentno = r.studentno

--RIGHT JOIN
SELECT s.studentno,studentname,subjectno,studentresult
FROM student AS s
RIGHT JOIN result AS r
ON s.studentno = r.studentno

--LEFT JOIN
SELECT s.studentno,studentname,subjectno,studentresult
FROM student AS s
RIGHT JOIN result AS r
ON s.studentno = r.studentno

SELECT s.studentno,studentname,subjectno,studentresult
FROM student AS s
RIGHT JOIN result AS r
ON s.studentno = r.studentno
WHERE studentresult IS NULL
```

> 自连接

就是将一张表拆分为两张表格，这在项目中十分的常见比如vue项目中的三级分类：category1ID,category2ID,category3ID。

举个例子：

1. 以下表是一个二级分类的表格

2. 第一层分类包含了：信息技术（categoryid=2）美术设计（categoryid=5）软件开发（categoryid=3）

3. 第二层分类包括了：办公信息（pid=2）web开发（pid=3）数据库（pid=3）ps技术（pid=5）

！[自连接分类表](images/4.PNG)

```mysql
SELECT `categoryName` AS '父栏目'，`categoryName` AS '子栏目'
FROM `category` AS a,`category` AS b
WHERE a.`categoryid` = b.`pid`
```

### 4.5分页和排序

> LIMIT 分页

作用：缓解数据库压力

LIMIT 起始值，页面大小

第一页：LIMIT 0,5 从第一条开始，每页显示五条数据

第二页：LIMIT 5,5 从第五条开始，每页显示五条数据

第N页：LIMIT (n-1)*pageSize,pageSize

> 排序ORDER BY 

- 升序 ASC

- 降序 DESC

```mysql
SELECT s.studentno,studentname,subjectno,studentresult
FROM student AS s
RIGHT JOIN result AS r
ON s.studentno = r.studentno
INNER JOIN `subject` sub
ON r.`SubjectNo` = sub.`SubjectNo`
WHERE subjectName = '数据库结构-1'
ORDER BY StudentResult DESC
LIMIT 0,5
```

### 4.6子查询

WHERE + 嵌套一个子查询语句

```mysql
--没有嵌套查询
SELECT studentno,r.studentname,studentresult
FROM result r
INNER JOIN `subject` sub
ON r.`SubjectNo` = sub.`SubjectNo`
WHERE subjectName = '数据库结构-1'
ORDER BY StudentResult DESC

--嵌套查询
SELECT studentno,r.studentname,studentresult
FROM result r
WHERE SubjectNo = (
    SELECT SubjectNo FROM subject
    WHERE SubjectName = '数据库结构-1'
)
ORDER BY StudentResult DESC
```

## MySQL函数

### 5.1常用函数

数学运算：

```mysql
SELECT ABS(-8) --绝对值
SELECT CEILING(9.4) --向上取整
SELECT FLOOR(9.4) --向下取整
SELECT RAND() --随机数
SELECT SIGN(-10) --判断数的符号，负数返回-1，正数返回1

--字符串函数
SELECT CHAR_LENGTH('NI') --字符串长度
SELECT CONCAT() -- 拼接字符串
SELECT INSERT() --替换，插入
SELECT LOWER() --转小写
SELECT UPPER() --转小写

--时间日期函数
SELECT CURRENT_DATE() --获取当前日期
SELECT CURDATE() --获取当前日期
SELECT NOW() --获取当前时间
SELECT LOCALTIME() --获取本地时间
SELECT SYSDATE() --系统时间
```

### 5.2聚合函数（常用）

函数名称:

- **COUNT()** 计数

- SUM() 求和

- AVG() 平均值

- MAX() 最大值

- MIN() 最小值

```mysql
SELECT COUNT(studentname) FROM student; --Count(字段)，会忽略所有的null值
SELECT COUNT(*) FROM student; --Count(*),不会忽略null值
SELECT COUNT(1) FROM result; --Count(1)，不会忽略所有的null值
```

## 6、事务

### 6.1、什么是事务

将一组SQL放在一个批次去执行。

事务原则：ACID原则

- 原子性：要么都成功要么都失败

- 一致性：事务处理前后数据完整性要保持一致

- 隔离性：在多个用户进行并发访问数据库时，各自事务互不影响

- 持久性：事务一旦提交则不可逆，被持久化到数据库中

整个事务流程如下：

```mysql
--mysql默认开启事务自动提交
SET autocommit = 0 /*关闭*/
SET autocommit = 1/*开启，（默认）*/


--手动处理事务流程
SET autocommit = 0 --关闭自动提交
--事务开启
START TRANSACTION --标记一个事务的开启，之后的sql都在一个事务里

INSERT XX
INSERT XX

--提交:持久化
COMMIT
--回滚：回到原来的样子：持久化后会失败！
ROLLBACK
--事务结束
SET autocommit = 1 --开启自动提交

--了解即可
SAVEPOINT 保存点名 --设置事务保存点
ROLLBACK TO SAVEPOINT 保存点名 --回滚到保存点
RELEASE SAVEPOINT 保存点名--撤销保存点
```

实际的案例：

```mysql
CREATE  DATABASE shop CHARACTER SET utf8 COLLATE utf8_general_ci
USE shop

CREATE TABLE `account`(
    `id` INT(3) NOT NULL AUTO_INCREMENT,
    `name` VARCHAR(30) NOT NULL,
    `money` DECIMAL(9,2) NOT NULL,
    PRIMARY KEY (`id`)
)ENGINE-INNDB DEFAULT CHARSET=utf8

INSERT INTO account(`name`,`money`)
VALUES(`A`,2000.00),(`B`,10000.00)

--模拟事务
SET autocommit = 0;
START TRANSACTION

UPDATE account SET money = money-500
WHERE `name` = A
UPDATE account SET money = money+500
WHERE `name` = B

COMMIT;--事务一旦被提交就无法回滚
ROLLBACK;
SET autocommit = 1;
```

## 7、索引

索引的本质是帮助SQL高效获取数据的数据结构算法。

### 7.1索引的分类

- 主键索引（PRIMARY KEY）：唯一的标识，不可重复，只能将一个列作为主键

- 唯一索引（UNIQUE KEY）：只是避免重复的列出现。但是其实可以将多个列标识为唯一索引

- 常规索引（KEY/INDEX）：默认索引，INDEX/KEY关键字设置

- 全文索引（FULLTEXT）：只在特定的引擎下有，快速定位数据

```mysql
--显示所有的索引信息
SHOW INDEX FROM student

--增加一个全文索引
ALTER TABLE school.student ADD FULLTEXT INDEX `studentName`(`studentName`)

--EXPLAIN 分析sql执行的情况
EXPLAIN SELECT * FROM student;--普通索引

EXPLAIN SELECT * FROM student WHERE MATCH(studentName) AGAINST('刘')
```

### 7.2索引的原则

- 不要对经常变动的数据加索引

- 小数据量不需要索引

- 索引一般加在用来查询的字段

> 索引的数据结构：BTREE（INNDB默认的数据结构）