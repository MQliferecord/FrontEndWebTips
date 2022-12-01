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