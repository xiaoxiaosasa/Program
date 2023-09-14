# SQL基础篇

## DDL

### DDL 数据库操作

- 查询

  - 查询所有数据库

    ```sql
    SHOW DATABASES;
    ```

  - 查询当前数据库

    ```sql
    SELECT DATABASES();
    ```

- 创建

  ```sql
  CREATE DATABASE [IF NOT EXISTS] 数据库名 [DEFAULT CHARSET 字符集] [COLLATE 排序规则];
  ```

- 删除

  ```sql
  DROP DATABASE [IF EXISTS] 数据库名;
  ```

- 使用

  ```sql
  	USE 数据库名;
  ```

  

### DDL 表操作

- 创建表

  ```sql
  CREATE TABLE 表名(
  	字段1 类型 [COMMENT 字段1注释],
  	...
  	字段n 类型 [COMMENT 字段n注释]
  ) [COMMENT 表注释]
  ```

- 查询当前数据库所有表

  ```sql
  SHOW TABLES;
  ```

- 查询表结构

  ```sql
  DESC TABLES;
  ```

- 查询指定表的建表语句

  ```sql
  SHOW CREATE TABLE 表名;
  ```



### DDL 数据类型

### DDL表操作

- 添加字段

  ```sql
  ALTER TABLE 表名 ADD 字段名 类型（长度） [COMMENT 注释] [约束];
  ```

- 修改字段

  ```sql
  -- 修改字段数据类型
  ALTER TABLE 表名 MODIFY 字段名 新数据类型（长度）;
  
  -- 修改字段名和字段类型
  ALTER TABLE 表名 CHANGE 旧字段名 新字段名 类型（长度） [COMMENT 注释] [约束];
  ```

- 修改表名

  ```sql
  ALTER TABLE 表名 RENAME TO 新表名;
  ```

- 删除表

  ```sql
  -- 删除表
  DROP TABLE [IF EXISTS] 表名;
  
  -- 删除表， 并重新创建该表
  TRUNCATE TABLE 表名;
  ```

  

## DML  数据操作语言

INSERT UPDATE DELETE

### DML 添加数据

- 给指定字段添加数据

  ```sql
  INSERT INTO TABLE(字段名1, 字段名2, ...) VALUES(值1, 值2, ...);
  ```

- 给全部字段添加数据

  ```sql
  INSERT INTO TABLE VALUES(值1, 值2, ...);
  ```

- 批量添加数据

  ```sql
  INSERT INTO TABLE(字段名1, 字段名2, ...) VALUES(值1, 值2, ...),(值1, 值2, ...),(值1, 值2, ...);
  
  INSERT INTO TABLE VALUES(值1, 值2, ...),(值1, 值2, ...),(值1, 值2, ...);
  ```

注意：

- 插入数据时， 指定的字段顺序需要与值的顺序是一一对应的
- 字符串和日期类型数据应该包含在引号中
- 插入的数据大小，应该在字段的规定范围内

### DML 修改数据

```sql
UPDATE TABEL SET 字段名1=值1, 字段名2=值2, ... [WHERE 条件];
```

### DML 删除数据

```sql
DELETE FROM TABLE [WHERE 条件];
```

注意：

- DELETE语句的条件可以有，也可以没有，如果没有条件，则会删除整张表的所有数据
- DELETE语句不能删除某一个字段的值（可以使用update）

## DQL 数据查询语言

语法

```sql
SELECT 
	字段列表
FROM
	表名列表
WHERE
	条件列表
GROUP BY
	分组字段列表
HAVING 
	分组后条件列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

### DQL 基本查询

- 查询多个字段

  ```sql
  SELECT 字段1,字段2, 字段3,...FROM TABLE;
  SELECT * FROM TABLE;
  ```

  

- 设置别名

  ```sql
  SELECT 字段1 [AS 别名1],字段2 [AS 别名2], ... FROM TABLE;
  ```

  

- 去除重复记录

  ```sql
  SELECT DISTINCT 字段列表 FROM TABLE;
  ```

### DQL条件查询

- 语法

  ```sql
  SELECT 字段列表 FROM TABLE WHERE 条件列表;
  ```

  

- 条件

  ```sql
  -- 比较运算符
  >
  >=
  <
  <=
  =
  <> 或 !=
  BETWEEN ... AND .. 在某个范围之内
  IN(...)
  LIKE 占位符 模糊匹配，_ 匹配单个字符， % 匹配任意个字符
  IS [NOT] NULL 是否为空
  
  --逻辑运算符
  AND 或 &&  并
  OR 或 ||   或
  NOT 或 !   非
  ```

  

### DQL聚合函数

- 将一列数据作为一个整体，进行纵向计算

  ```sql
  SELECT 聚合函数(字段列表) FROM TABLE;
  ```

  

- 常见的聚合函数

  ```sql
  count
  max
  min
  avg
  sum
  ```

注意：

​	null值不参与聚合运算

### DQL分组查询

- 语法

  ```sql
  SELECT 字段列表 FROM 表名 [WHERE 条件] GROUP BY 分组字段名 [HAVING 分组后过滤条件];
  ```

  

- where和having的区别

  - 执行时机不同：where是分组之前进行过滤，不满足where条件不参与分组；而having是分组之后对结果进行过滤
  - 判断条件不同：where不能对聚合函数进行判断，而having可以

### DQL 排序查询

- 语法

  ```sql
  SELECT 字段列表 FROM TABLE ORDER BY 字段1 排序方式1, 字段2 排序方式2;
  ```

- 排序方式

  - ASC 升序
  - DESC 降序

  如果是多字段排序，当第一个字段值相同时，才会根据第二个字段进行排序。

### DQL分页查询

- 语法

  ```sql
  SELECT 字段列表 FROM 表名 LIMIT 起始索引,查询记录数
  ```

  注意：

  - 起始索引从0开始，起始索引 = （查询页码 -1）*每页显示的记录数
  - 分页查询是数据库的方言，不同的数据库有不同的实现，mysql中是limit
  - 如果查询的是第一页数据，起始索引可以省略，直接简写为limit 10

  

### DQL执行顺序

```
FROM
	
WHERE

GROUP BY

HAVING

SELECT

ORDER BY

LIMIT

```

## DCL 数据控制语言

管理数据库用户、控制数据库的访问权限

### DCL管理用户

- 查询用户

  ```mysql
  -- 用户信息在mysql数据库中的user表里
  USE mysql; 
  SELECT * FROM user;
  ```

  

- 创建用户

  ```mysql
  CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
  
  -- 只允许本机访问
  create user 'itcast'@'localhost' identified by '123456';
  
  -- 允许任何机器访问
  create user 'heima'@'%' identified by '123456'
  ```

  

- 修改用户密码

  ```mysql
  ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
  ```

  

- 删除用户

  ```mysql
  DROP USER '用户名'@'主机名';
  ```

  

### DCL权限控制

```mysql
权限
ALL,ALL PRIVILEGES  所有权限
SELECT              查询数据
INSERT              
UPDATE
DELETE
ALTER			    修改表
DROP				删除数据库/表/视图
CREATE				创建数据库/表
```

- 查询权限

  ```mysql
  SHOW GRANTS FOR '用户名'@'主机名';
  ```

  

- 授予权限

  ```mysql
  GRANT 权限列表 ON 数据库名.表名 TO '用户名'@'主机名';
  
  -- 授予heima用户itcast数据库所有表的所有权限
  grant all on itcast.* to 'heima'@'%';
  ```

  

- 撤销权限

  ```mysql
  REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
  ```

  

## 函数

### 字符串函数

```
CONCAT(S1, S2, .., Sn)    字符串拼接
LOWER(str)
UPPER(str)
LPAD(str, n, pad) 左填充，用pad字符串对str左边进行填充 ，达到n个字符串长度
RPAD(str, n, pad) 右填充
TRIM(str)   去掉头部和尾部的空格
SUBSTRING(str, start, len) 截取
```

### 数值函数

```
CEIL(x)
FLOOR(x)
MOD(x, y)
RAND() 随机数
ROUND(X, Y) 对x四舍五入，保留y位小数
```

### 日期函数

```mysql
CURDATE()
CURTIME()
NOW()
YEAR(date)
MONTH(date)
DAY(date)
DATE_ADD(date, INTERVAL expr type)  返回日期时间值加上一个type时间间隔expr后的时间值
DATEDIFF(date1, date2)  返回起始时间date1和date2之间的天数
```

### 流程控制函数

```mysql
IF(value, t, f)             如果value为true，返回t，否则返回f
IFNULL(value1, value2)      如果value不为空，返回value1, 否则返回value2
CASE WHEN [val1] THEN [res1] ... ELSE [default] END 如果val1为true，返回res1 ...否则返回默认值
CASE [expr] WHEN [val1] THEN [res1] ...ELSE [default] END
```

## 约束

### 概述

- 作用于表中字段上的规则， 用于限制存储在表中的数据

- 目的是保证数据库中的数据的正确性、有效性和完整性

- 分类

  - 非空约束：限制该字段 不能为空   NOT NULL
  - 唯一约束：保证该字段唯一不重复 UNIQUE
  - 主键约束：主键是一行数据的唯一标志要求非空且唯一  PROMARY  KEY
  - 默认约束：保存数据时，如果未指定该字段的值，采用默认值  DEFAULT
  - 检查约束：保证字段满足某一个条件 CHECK
  - 外键约束：用来让两张表的数据之间建立连接，保证数据的一致性和完整性 FOREIGN KEY

  ```mysql
  create table emp(
  	id int primary ket auto_increment;
      ...
  );
  
  
  create table user(
  	id int primary key auto_increment comment '编号',
  	name varchar(10) not null unique comment '姓名',
  	age int check (age > 0 && age <=120) comment '年龄',
  	status char(1) default '1' comment '状态',
  	gender char(1) comment '性别',
      dept_id int foreign key references emp(id) 
  ) comment '用户表';
  ```



- 添加外键

  - 方式一

      ```mysql
      CREATE TABLE 表名(
        字段名 数据类型,
        ...
        [CONSTRAINT] [外键名称] FOREIGN KEY(外键字段名) REFERENCES 主表(主表列名)
      );
      ```

  - 方式二 为已经创建的表添加外键

    ```mysql
    ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键字段名) REFERENCES 主表(字段名);
    ```

- 删除外键

  ```mysql
  ALTER TABLE DROP FOREIGN KEY 外键名称;
  ```

- 外键删除更新行为

  - NO ACTION ： 当在父表中删除、更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新，与RESTRICT一致

  - RESTRICT: 

  - CASCADE: 当在父表中删除更新记录时，首先检查该记录是否有对应外键，如果有则也删除更新外键子表中的记录

  - SET NULL：当在父表中删除更新记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（这就要求该外键允许取null）

  - SET DEFAULT： 父表变更时，子表将外键列设置成一个默认的值（Innodb不支持）

  - 语法

    ```mysql
    alter table emp add constraint fk_emp_dept_id foreign key (dept_id) references dept(id) on update cascade on delete cascade;
    ```
## 多表查询

###   概述

项目开发中，在进行数据库表结构设计时，会根据业务需求及业务模块之间的关系，分析并设计表结构，由于业务之间相互关联，所以各个表结构之间也 存在着各种联系，基本上分为三种：

- 一对多（多对一）

  一个部门有多个员工

  - 实现 在多的一方建立外键，指向一的一方的主键

- 多对多

  学生和课程的关系

  - 实现：建立第三张中间表，中间表至少包含两个外键 分别关联两方主键

- 一对一

  用户与用户详情的关系

  用于单表拆分，将一张表的基础字段放在一张表中，其他详情字段放在另一张表中，以提升操作效率

  - 实现：在任意一方加入外键关联另一方的主键 ，并且设置外键为唯一的(UNIQUE)

- 多表查询分类

  - 连接查询
    - 内连接
    - 左外连接
    - 右外连接
    - 自连接：当前表和自身的连接查询，自连接必须使用表别名
  - 子查询

### 连接查询

#### 内连接

获取的结果是两张表的交集

- 隐式内连接

  ```
  select 字段列表 from 表1,表2 where 条件...;
  ```

  

- 显示内连接

  ````
  select 字段列表 from 表1 inner join 表2 on 连接条件...;
  ````

#### 左外连接

相当于查询左表的所有数据  包含表1和表2交集部分的数据

```
select 字段列表 from table1 left [outer] join table2 on 条件...;
```



#### 右外连接

相当于查询右表的所有数据  包含表1和表2交集部分的数据

```
select 字段列表 from table1 right [outer] join table2 on 条件...;
```

#### 自连接

```mysql
-- 查询员工及其所属领导的名字
select a.name, b.name from emp a, emp b where a.managerid = b.id;
```

### 联合查询

就是把多次查询的结果合并起来，形成一个新的查询结果集

```sql
-- 将薪资低于5000的员工和年龄大于50的员工全部查询出来
select * from emp where salary < 5000
union all
select * form emp where age > 50;

-- 去重

select * from emp where salary < 5000
union
select * form emp where age > 50;
```

联合查询的多张表的列数必须保持一致，字段类型也需要保持一致

union all 会将全部的数据直接合并在一起， union会对合并之后的数据去重

### 子查询

sql语句中嵌套select语句，称为嵌套查询又称子查询

- 根据查询结果不同分类
  - 标量子查询  子查询返回的结果是单个值   = <> , < , >, <=.....
  
    ```
    -- 查询销售部的所有员工
    select * from emp where dept_id=(select id from dept where name='销售部');
    ```
  
    
  
  - 列子查询 in any some all  子查询返回的结果是一列（可以是多行）
  
    ```
    -- 查询销售部和市场部的所有员工信息
    select * from emp where dept_id in (select id from dept where name='销售部' or name='市场部');
    ```
  
    
  
  - 行子查询   子查询返回的结果是一行（可以是多列）= 、<>、 in、 not in 
  
    ```mysql
    -- 查询与张无忌的薪资及直属领导相同的员工信息
    select * from emp where (salary, managerid) = (select salary, managerid from emp where name='张无忌');
    
    ```
  
    
  
  - 表子查询   子查询返回的结果是多行多列
  
    ```mysql
    -- 查询职位和薪资和鹿杖客、宋远桥相同的员工信息
    select * from emp where (job, salary) in (select job, salary from emp where name='鹿杖客' or name='宋远桥')
    ```
  
- 根据子查询的位置分类
  - where之后
  - from之后
  - select之后

## 事务

事务是一组操作的集合，他是一个不可分割的工作单位，事务会把所有操作作为一个整体一起向系统提交或者撤销操作请求，即这些操作要么同时成功，要么同时失败。

- 查看/设置事务提交方式

  ```sql
  SELECT @@autocommit;
  --设置为手动提交
  SET@@autocommit=0; 
  ```

- 提交事务

  ```
  commit
  ```

- 回滚事务

  ```
  rollback
  ```

### 事务操作

- 开启事务

  ```
  start transaction;
  ```

- 提交事务

- 回滚事务

### 事务的四大特性

- 原子性：事务是不可分割的最小操作单元，要么全部成功，要么全部失败
- 一致性：事务完成时，必须使所有的数据都保持一致状态
- 隔离性：数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行
- 持久性：事务一旦提交或者回滚，它对数据库中的数据的改变是永久的

### 并发事务问题

- 脏读：一个事务读到另一个事务还没有提交的数据
- 不可重复读：一个事务先后读取同一条记录，但两次读取的数据不同，称之为不可重复读
- 幻读：一个事务按照条件查询数据时，没有对应的数据行，但是在插入数据时，又发现这行数据已经存在，好像出现了幻影

### 事务的隔离级别

- read uncommitted   三个事务问题都会有
- read committed       不会出现脏读问题
- repeatable read （默认）会出现幻读
- serializable   都不会出现

- 查看事务的隔离级别

  ```
  select @@transaction_isolation
  ```

  

- 设置事务的隔离级别

  ```
  set [session|global] transaction isolation level {read uncommited|read commited|repeated read|serialzable}
  ```

  **数据库的隔离级别越高，数据越安全，但是性能越低。**

# MySQL进阶篇

## mysql体系结构

- 连接层
- 服务层
- 引擎层
- 存储层



## 存储引擎

存储引擎就是存储数据、建立索引、更新、查询数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也被称为表类型。

- 创建表示，指定存储引擎

  ```
  create table tb(
  	field1 type comment '注释',
  	....
  ) engine = INNODB [comemnt '注释'];
  ```

  

- 查看当前数据库支持的存储引擎

  ```
  show engines;
  ```

### 存储引擎特点

1. InnoDB

   - 介绍

     innodb是一种兼顾高可靠性和高性能的通用存储引擎，在mysql 5.5之后，innodb是默认的mysql存储引擎

   - 特点

     - DML操作遵循ACID模型，支持**事务**
     - **行级锁**， 提高并发访问性能
     - 支持**外键**约束，保证数据的正确性和完整性

   - 文件

     xxx.ibd   xxx代表表名， innodb引擎的每张表都会对应这样一个表空间文件，存储该表的表结构、数据、索引

     参数：innodb_file_per_table

   - 逻辑存储结构

     tablespace表空间中很多segment段，每个段中很多区extent（1M），每个区中很多page页（16k），每个页中很多行row

     

2. MyISAM

   - 介绍

     MyISAM是MySQL早期默认的存储引擎

   - 特点

     不支持事务，不支持外键

     支持表锁，不支持行锁

     访问速度快

   - 文件

     xxx.sdi:存储表结构

     xxx.MYD：存储数据

     xxx.MYI：存储索引

3. Memory

   - 介绍

     Memory引擎的表数据是存储在内存中的，由于受到硬件问题、断电问题的影响，只能将这些表作为临时表或者缓存使用

   - 特点

     内存存放

     hash索引（默认）

   - 文件

     xxx.sdi： 存储表结构信息

   

## 索引

- 介绍

  索引是帮助musql高效获取数据的数据结构（有序）。

- 优缺点

  优势：提高数据检索的效率，降低数据库的IO成本；通过索引列对数据进行排序，降低数据排序的成本，降低cpu的消耗

  劣势：索引也是要占空间的；索引大大提高了查询效率，同时也降低了更新表的速度，如对表进行insert、update、delete时，效率降低

### 索引结构

MySQL的索引在存储引擎层实现的，不同的存储引擎有不同的结构，主要包含一下几种：

- B+Tree索引  最常见的索引类型，大部分的数据库引擎都支持
- Hash索引  底层数据结构是用哈希实现的，只有精确匹配索引列的查询才有效，不支持范围查询
- R-Tree空间索引  是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少
- Full-text 全文索引 是一种通过建立倒排索引，快速匹配文档的方式。类似于Lucene，Solr， Es

### 索引分类

- 主键索引 针对表中主键创建的索引，默认自动创建，只能有一个，关键字primary
- 唯一索引 避免同一个表中某数据列中的值重复   可以有多个   关键字 unique
- 常规索引 快速定位特定数据    可以有多个  
- 全文索引  全文索引查找的是文本中的关键词，而不是比较索引中的值 可以有多个， 关键字fulltext





在innodb中，根据索引的存储形式，分为以下两种：

- 聚集索引

  将数据存储与索引放到了一块，索引结构的叶子节点保存了行数据    特点： 必须有而且只有一个

- 二级索引

  将数据与索引分开存储，索引结构的叶子结点关联的是对应的主键    特点：可以存在多个

1. 聚集索引的选取规则：

   - 如果存在主键，主键索引就是聚集索引
   - 如果不存在主键，将使用第一个唯一索引作为聚集索引
   - 如果没有主键和唯一索引，则innodb会自动生成一个rowid作为隐藏的聚集索引

   ![image-20230615155427851](C:\Users\47264\AppData\Roaming\Typora\typora-user-images\image-20230615155427851.png)

### 索引语法

- 创建索引

  ```sql
  create [unique|fulltext] index index_name on table_name (index_col_name,...);
  
  
  -- 示例
  create index idx_user_name on user(name);
  create unique index idx_user_phone on user(phone);
  -- 创建联合索引
  create index idx_user_pro_age_sta on user(profession, age, status);
  ```

  

- 查看索引

  ```
  show index from table_name;
  ```

  

- 删除索引

  ```
  drop index index_name on table_name;
  ```

  

### sql性能分析

- sql的执行频率

  show [session|global] status 命令查看服务器状态信息

  ```
  show global staus like 'Com_______';
  
  着重看增删改查频次即可
  ```

  只能分析出select执行频次最多

- 慢查询日志

  慢查询日志记录了所有执行时间超过指定参数（long_query_time， 单位s，默认10s）的所有sql语句的日志

  ```sql
  -- 查看慢查询是否开启
  show variables like 'slow_query_log';
  ```

  

  mysql的慢查询日志默认没有开启，需要再mysql的配置文件（/etc/my.cnf）中配置如下信息

  ```
  # 开启慢日志查询开关
  slow_query_log = 1
  
  # 设置慢日志时间为2秒，sql语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
  long_query_time=2
  ```

  配置完成之后，重新启动mysql进行测试，查看慢日志文件记录的信息 /var/lib/mysql/localhost-slow.log

- profile详情

  show profiles能够在做sql优化时帮助我们了解时间都消耗到哪里去了。通过have_profiling参数，能够看到当前mysql是否支持profile操作

  ```
  select @@have_profiling  查看是否支持
  
  select@@profiling  查看开关是否打开
  
  set profiling=1  打开
  ```

  打开之后执行一系列sql

  然后用**show profiles**命令就能查看刚执行的命令耗时

  ```
  -- 查看每一条sql的耗时基本情况
  show profiles;
  
  -- 查看指定query_id的sql语句各个阶段的耗时情况.query_id来自show profiles命令
  show profile for query query_id; 
  
  
  --查看指定query_id的sql语句的CPU使用情况
  show profile cpu for query query_id;
  ```

- explain执行计划

  explain或者desc命令获取mysql如何执行Select语句的信息，包括在select语句执行过程中表如何连接和连接的顺序。

  语法：直接在select语句之前加上关键字explain

  explain 执行计划各字段含义

  - id select查询的序列号，表示查询中执行select子句或者操作表的顺序（id相同，执行顺序从上到下；id不同，值越大，越先执行）

  - select_type

    表示select的类型，常见的取值有simple（简单表，即不使用表连接或者子查询）、primary（主查询，外层的查询）、union（union的第二个或者后边的查询语句）、subquery（子查询）等

  - type

    表示连接类型，性能由好到差的连接类型为null、system、const、eq_ref、ref、index、all

  - possible_key

    可能应用到表上的索引，一个或多个

  - key 实际用到的索引

  - key_len  索引长度

  - rows  执行查询的行数

  - filtered 返回结果的行数占需读取行数的百分比，filter的值越大越好


### 索引使用

#### 最左前缀法则

如果索引了多列（联合索引），要遵守最左前缀法则。最左前缀法则指的是查询从索引的最左列开始，并且不跳过索引中的列。如果索引最左边的列不存在，索引将全部失效；如果跳过某一列，索引将部分失效（后面的字段索引失效）

```
-- 联合索引 profession、age、status
explain select * from tb_user where profession='软件工程' and age=31 and status='0';

explain select * from tb_user where profession='软件工程' and age=31;

explain select * from tb_user where profession='软件工程';

explain select * from tb_user where age=31 and status='0';  索引失效

explain select * from tb_user where status='0'; 索引失效


```

#### 范围查询

联合索引中，出现范围查询（>，<），范围查询右侧的列索引失效

```sql
-- status 失效
explain select * from tb_user where profession='软件工程' and age > 30 and status='0';

-- >= <=索引不失效
explain select * from tb_user where profession='软件工程' and age >= 30 and status='0';
```

#### 索引列运算

不要在索引列上进行运算操作否则索引将失效

```sql
explain select * from tb_user where substring(phone, 10, 2) = '15';
```

#### 字符串不加引号

字符串类型字段使用时，不加引号，索引将失效

```
explain select * from tb_user where profession='软件工程' and age=31 and status=0;

explain select * from tb_user where phone = 128467888;
```

#### 模糊查询

如果仅仅是尾部模糊匹配，索引不会失效。如果是头部模糊匹配，索引失效。

```sql
explain select * from tb_user where profession like '软件%';

-- 失效
explain select * from tb_user where profession like '%工程';

-- 失效
explain select * from tb_user where profession like '%件%';
```

#### or

or分割开的条件，如果前后有一个列没有索引，那么涉及的索引不会被用到

```sql
explain select * from tb_user where id=10 or age=23;

explain select * from tb_user where phone='12346787' or age=23;

-- 由于age没有索引，所以即使id、phone有索引，索引也会失效。需要对age建立索引
```

#### 数据分布影响

如果mysql评估使用索引比全表扫描更慢，则不使用索引。

```sql
select * from tb_user where phone >= '177...000';

select * from tb_user where phone >= '177...015';


explain select * from tb_user where profession is null;
```

#### SQL提示

SQL提示是优化数据库的一个重要手段，就是在sql语句中加入一些人为的提示来达到优化操作的目的

- use index

  ```
  explain select * from tb_user use index(index_user_pro) where profession='软件工程';
  ```

  

- ignore index

  ```
  explain select * from tb_user ignore index(index_user_pro) where profession='软件工程';
  ```

  

- force index

  ```
  explain select * from tb_user force index(index_user_pro) where profession='软件工程';
  ```



#### 覆盖索引

尽量使用覆盖索引（查询使用了索引，并且需要返回的列，在该索引中已经能全部找到），减少 select *

```sql
-- profession, age, status是联合索引，id是二级索引挂着的数据
explain select id, profession, age, status from tb_user where profession='软件工程' and age=31 and status='0';

-- name 需要到聚集索引中找到叶子节点才能获取到
explain select id, profession, age, status.name from tb_user where profession='软件工程' and age=31 and status='0';
```



using index condition: 查找使用了索引，但是需要回表查询数据，即需要到聚集索引中查找

using where; using index: 查找使用了索引，但是需要的数据在索引列中都能找到，所以不需要回表查询数据

#### 前缀索引

当字段类型为字符串（varchar，text）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时，浪费很大的磁盘IO，影响查询效率。此时可以将字符串的一部分前缀建立索引，这样可以大大节约索引空间，从而提高索引效率。

- 语法

```
create index idex_xxx on table_name(column(n));
```

- 前缀长度

- 可以根据索引的选择性来决定，而选择性是指不重复的索引值（基数）劲儿数据表的记录总数的比值，索引选择性越高则查询效率越高。

  唯一索引的选择性是1，这是最好的选择性。

  ```
  select count(distinct email)/count(*) from tb_user;
  
  select count(distinct substring(email, 1, 5))/count(*) from tb_user;
  ```

  

#### 单列&联合索引

单列索引：即一个索引只包含单个列

联合索引：即一个索引包含了多个列

在业务场景中，如果存在多个查询条件，考虑针对查询字段建立索引时，建议建立联合索引，而非单列索引。

### 索引的设计原则

1. 针对数据量较大的，且查询比较频繁的表建立索引
2. 针对于常作为查询条件where、排序order by、分组group by操作的字段建立索引
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用索引的效率越高
4. 如果是字符串类型的的字段，字段的长度较长，可以针对字段的特点，建立前缀索引
5. 尽量使用联合索引，减少单列索引，查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表，提高查询效率
6. 要控制索引的数量，索引并不是越多越好，索引越多，维护索引的结构的代价也就越大，会影响增删改的效率
7. 如果索引列不能存储null值，请在创建表的时候使用not null约束他。当优化器知道每列是否包含null值时，他可以更好地确定哪个索引最有效的用于查询。



## SQL优化

### 插入数据

#### insert优化

- 批量插入
- 手动事务提交
- 主键顺序插入

- 大批量数据插入时

  如果一次性需要插入大批量数据，使用insert语句插入性能较低，此时可以使用mysql数据库提供的load指令进行插入。

```sql
-- 客户端连接服务器时加上参数 --local-infile
mysql --local-infile -u root -p

-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
select @@local_infile  
set global local_infile = 1;

--执行load指令将准备好的数据，加载到表结构中
load data local infile '/root/sql1.sql' into table `tb_user` fields terminated by ',' lines terminated by '\n';

```



### 主键优化

- 数据组织方式

  在innodb存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表。

- 页分裂

  主键乱序插入时，如果id应该插入的页已经满了，会分裂出一部分数据重新组织安排页顺序

- 页合并

  当删除一行记录时，实际上并没有被物理删除，只是被标记为删除并且他的空间变得允许被其他记录使用

  当页中的删除的记录达到merge_threshold(默认为50%)， innodb会开始寻找最近的页（前或后）看看是否可以将两个页合并以优化空间使用

#### 主键设计原则

1. 满足业务需求的情况下，尽量降低主键的长度
2. 插入数据时，尽量选择顺序插入，选择使用auto_increment自增主键
3. 尽量不要使用UUID做主键或者其他自然主键，如身份证号
4. 业务操作时，避免对主键的修改

### order by 优化

1. using filesort：通过表的索引或者全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作，所有不是通过索引直接返回排序结果的排序叫filesort排序
2. using index：通过有序索引顺序扫描直接返回有序数据，这种情况即为using index， 不需要额外排序，操作效率高

```sql
-- 没有创建索引时，根据age、phone进行排序
explain select id, age, phone from tb_user order by age, phone;

-- 创建索引  默认索引是按照age asc和phone asc进行升序排序
create index idx_user_age_phone_aa on tb_user(age, phone);
explain select id,age,phone from tb_user order by age, phone;

-- 降序
create index idx_user_age_phone_ad on tb_user(age asc, phone desc);
explain select id, age, phone from tb_user order by age acs, phone desc;


```

- 根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则

- 尽量使用覆盖索引

- 多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC DESC）

- 如果不可避免的初夏filesort， 大数据量排序时，可以适当增加缓冲区的大小sort_buffer_size（默认256k）

  ```
  show variables like 'sort_buffer_size';
  ```

### group by优化

- 在分组操作时，可以通过索引来提高效率
- 分组操作时，索引的使用也是满足最左前缀法则的

### limt优化

一个非常头疼的问题就是limit 2000000， 10 ，  此时需要MySQL排序前2000010记录， 仅仅返回2000000~2000010的记录，其他的丢弃，查询的代价非常大。

```sql
select s.* from tb_user s, (select id from tb_user order by id limit 2000000, 10) a where s.id=a.id;
```

优化思路： 一般分页查询时，通过创建覆盖索引能够较好的提高性能，可以通过**覆盖索引加子查询**的形式进行优化。

### count优化

```
explain select count(*) from tb_user;
```

- MyISAM 引擎把一个表的总行数存在了磁盘上，因此计算表的行数会直接返回这个数，效率很高；但是不能有where条件，带条件查询就不是总行数了
- innodb引擎就麻烦了，他执行count（*）的时候，需要把数据一行一行地从引擎里读出来，然后累积计数

优化思路：自己计数，借助redis等

count的几种用法：

- count(主键)

  innodb引擎会遍历整张表，把每一行主键id都取出来，返回给服务层。服务层拿到主键后，直接按行进行累加（主键不可能为null）

- count(字段)

  没有not null 约束： innodb引擎会遍历整张表把每一行字段值都取出来，返回给服务层，服务层判断是否为null，不为null，计数累加。

  有not null约束：innodb引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加

- count(1)

  innodb引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字1进去，直接按行进行累加

- count(*)

  innodb引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加

  按照效率的话，count(字段) < count(主键id) <count(1) ~count(\*), 所以尽量使用count(\*)

### update优化

分别在俩客户端先后开启事务执行下列语句

```
update student set no='20000000002211' where id=1;

update student set no='20000000002211' where name='韦一笑';
```

innodb的行锁是针对索引加的锁，不是针对记录加的锁，并且该索引不能失效，否则会从行级锁升级为表锁。一旦表被锁住，将降低并发性能。所以应该尽量根据主键、索引字段进行数据更新。

## 视图

### 介绍

​		视图是一种虚拟存在的表，视图中的数据并不在数据库中真实存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。

​		通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果，所以我们创建视图的时候，主要的工作就落在创建这条SQL查询语句。

- 创建视图

```sql
CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT 语句 [WITH [CASCADED|LOCAL] CHECK OPTION]
```

- 查询视图

  ```
  -- 查看创建视图语句
  show create view 视图名;
  
  --查看视图数据
  select * from 视图名称 where...
  
  ```

- 修改视图

  ```
  -- 方式1
  CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT 语句 [WITH [CASCADED|LOCAL] CHECK OPTION]
  
  --方式2
  ALTER VIEW 视图名称[(列名列表)] AS SELECT 语句 [WITH [CASCADED|LOCAL] CHECK OPTION]
  ```

- 删除视图

  ```
  DROP VIEW [IF EXISTS] 视图名称 [, 视图名称];
  ```

  

### 视图的检查选项

​		当使用WITH CHECK OPTION子句创建视图时，MySQL会通过视图检查正在更换的每个行，例如插入、更新、删除，以使其符合视图的定义。MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。为了确定检查的范围，mysql提供了两个选项，CASCADED（级联）和LOCAL（）,默认为CASCADED.

```sql
create view v1 as select id, name from student where id<=20;
create view v2 as select id, name from v1 where id >= 10 with cascaded check option;
```

### 视图的更新

要使视图更新，视图中的行与基础表中的行之间必须存在一对一的关系。如果视图函数包含以下任何一项，则该视图不能更新：

1. 聚合函数或者窗口函数
2. distinct
3. group by
4. having
5. union 或者 union all

### 视图作用

1. 简单

   视图不仅可以简化用户对数据的理解，也可以简化他们的操作。那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件

2. 安全

   数据库可以授权，但不能授权到数据库特定的行和特定的列上。通过视图用户只能查询和修改他们所能见到的数据

3. 数据独立

   视图可以帮助用户屏蔽真实表结构变化带来的影响。  真实表字段修改只需要将字段加as 原字段名字，view查询的字段不用做修改

## 存储过程

### 介绍

存储过程是事先经过编译并存储在数据库中一段SQL语句的集合，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。

存储过程思想上很简单 ，就是数据库sql语言层面的代码封装与重用。

### 特点

封装、复用

可以接收参数，也可以返回数据

减少网络交互，效率提升

### 使用

- 创建

  ```
  create procedure 存储过程名称([参数列表])
  begin
  	-- SQL语句
  end;
  
  
  
  create procedure p1()
  begin
  	select count(*) from student;
  end
  
  
  call p1();
  ```

  

- 调用

  ```
  call 名称([参数]);
  ```

- 查看

  ```
  -- 查询指定数据库xxx存储过程及状态信息
  select * from information_schema.routines where routine_schema='xxx';
  
  -- 查询某个存储过程的定义
  show create procedure 存储过程名称;
  
  ```

  

- 删除

  ```
  drop procedure [if exists] 存储过程名称;
  ```

- 注意

  在命令行中， 执行创建存储过程的SQL时，需要通过关键字delimiter指定sql语句的结束符。

  如 delimiter $$; 之后的语句结束就是以$$为标志

### 变量

#### 系统变量

是MySQL服务器提供，不是用户定义的，属于服务器层面。分为全局变量(global)、会话变量（session）

- 查看系统变量

  ```sql
  -- 查看所有系统变量
  SHOW [SESSION | GLOBAL] VARIABLES;
  -- 通过模糊查询方式寻找变量
  SHOW [SESSION | GLOBAL] VARIABLES LIKE '...';
  
  -- 查看指定变量
  SELECT @@[SESSION | GLOBAL] 系统变量名;
  ```

  

- 设置系统变量

  ```sql
  SET [SESSION | GLOBAL] 系统变量名 = 值;
  
  SET @@[SESSION | GLOBAL] 系统变量名 = 值;
  ```

- 注意：

  如果没有指定session、global， 默认是session，会话变量

  mysql服务重启之后，所有设置的全局参数会失效，要想不失效， 可以在/etc/my.conf中配置。

#### 用户自定义变量

是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用 @变量名 使用就可以。其作用域为当前连接。

- 赋值

  ```
  SET @var_name = expr [, @var_name= expr];
  SET @var_name := expr [, @var_name= expr];
  
  SELECT @var_name := expr [, @var_name :=expr];
  SELECT 字段名 INTO @var_name FROM 表名;
  ```

  

- 使用

  ```
  SELECT @var_name;
  ```

- 用户定义的变量无需对其进行声明或初始化，只不过获取到的值为null。

#### 局部变量

局部变量是根据需要定义的在局部生效的变量，访问之前需要declare 声明。可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的begin...end块。

- 声明

  ```
  DECLARE 变量名 变量类型 [DEFAULT...];
  ```

  变量类型就是数据库字段类型

- 赋值

  ```
  SET 变量名 = 值;
  SET 变量名 := 值;
  SELECT 字段名 INTO 变量名 FROM 表名;
  ```

  ```
  create procedure p2()
  begin
  	declare stu_count int default 0;
  	select count(*) into stu_count from student;
  	select stu_count;
  end;
  
  call p2();
  ```

### if 判断

```
create procedure p3()
begin
	declare score int default 58;
	declare result varchar(10);
	
	if score >= 85 then
		set result := '优秀';
	elseif score >= 60 then
		set result := '及格';
	else 
		set result := '不及格'
	endif;
	
	select result;
end;

call p3();
```



### 参数

类型

```
IN        该参数作为输入，也就是需要调用时传入值
OUT		  该参数作为输出，也就是该参数可以作为返回值
INOUT	  既可以作为输入也可以作为输出参数
```

- 用法

  ```sql
  CREATE PROCEDURE 存储过程名称([IN/OUT/INOUT] 参数名 参数类型)
  BEGIN
  	SQL
  END;
  ```

- 示例

  ```sql
  create procedure p4(in score int, out result varchar(10))
  begin
  	
  	if score >= 85 then
  		set result := '优秀';
  	elseif score >= 60 then
  		set result := '及格';
  	else 
  		set result := '不及格'
  	endif;
  end;
  
  
  call p4(68, @result);
  select @result;
  ```

  

### case

- 语法一

  ```sql
  CASE case_value
  	WHEN when_value1 THEN statement_list1
  	[WHEN when_value2 THEN statement_list2]...
  	[ELSE statement_list]
  END CASE;
  ```

- 语法二

  ```mysql
  CASE
  	WHEN search_conditon1 THEN statement_list1
  	[HEN search_conditon2THEN statement_list2]..
  	[ELSE statement_list]
  END CASE;
  ```

- 举例

  ```mysql
  create procedure p6(in month int)
  begin
  	declare result varchar(10)
  	
  	case
  		when month >= 1 and month <= 3 then
  			set result := '第一季度';
  		when month >= 4 and month <= 6 then
  			set result := '第二季度';
  		when month >= 7 and month <= 9 then 
  			set result := '第三季度'
  		when month >= 10 and month <= 12 then
  			set result := '第四季度';
  		else
  			set result := '非法参数';
  	end case;
  	
  	select concat('您输入的月份为：', month, '所属的季度为：', result);
  ```

### while

- 语法

  ```mysql
  WHILE 条件 DO
  	SQL逻辑...
  END WHILE;
  ```

```
create procedure p7(in n int)
begin
	declare total int default 0;
	while n > 0 do
		set total := total + n;
		set n := n -1;
	end while;
	
	select total;
end;

call p7(10);
```



### repeat

repeat是有条件的循环控制语句，当满足条件的时候退出循环。

```
REPEAT
	SQL逻辑
	UNTIL 条件
END REPEAT
```

举例 1累加到n

```mysql
create procedure p8(in n int)
begin
	declare total int default 0;
	repeat
		set total := total + n;
		set n := n -1;
		util n=0
	end repeat;
	select total;
end;

call p8(10);
		
```

### loop

loop实现简单的循环，如果不在sql逻辑中增加退出 循环条件，可以用其来实现简单的死循环。loop可以配合一下两个语句使用

- LEAVE: 配合循环使用， 退出循环
- ITERATE: 必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环

```
[begin_label:] LOOP
	SQL逻辑
END LOOP [end_label];
```

- 示例

  ```
  -- 1到n累加
  create procedure p9(in n int)
  begin
  	declare total int default 0;
  	
  	sum:loop
  		if n <=0 then
  			leave sum;
  		end if;
  		
  		set total := total + n;
  		set n := n - 1;
  	end loop sum;
  	
  	select total;
  end;
  
  call p9(10);
  
  -- 计算 1到n的偶数累加结果
  
  create procedure p10(in n int)
  begin
  	declare total int default 0;
  	
  	sum:loop
  		if n <=0 then
  			leave sum;
  		end if;
  		
  		if n % 2 =1 then
  			set n := n - 1;
  			iterate sum;
  		end if;
  		
  		set total := total + n;
  		set n := n - 1;
  	end loop sum;
  	
  	select total;
  end;
  
  
  
  ```
### 游标

游标cursor是用来存储查询结果集的数据类型，在存储过程和函数中可以使用游标对结果集进行循环处理。游标的使用包括游标的声名、open、fetch、close，其语法分别如下

- 声明游标

  ```sql
  declare 游标名称 cursor for 查询;
  ```

  

- 打开游标

  ```
  open 游标名称
  ```

  

- 获取游标记录

  ```
  fetch 游标名称 into 变量[,变量];
  ```

  

- 关闭游标

  ```
  close 游标名称;
  ```

- 条件处理程序

  条件处理程序handler可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤。

  ```
  declare handler_action handler for condition_value[,condition_value]... statement;
  
  handler_action
  	continue: 继续执行当前程序
  	exit： 终止执行当前程序
  
  condition_value
  	SQLSTATE sqlstate_value: 状态码，如02000
  	SQLWARNING: 所有以01开头的SQLSTATE代码的简写
  	NOT FOUND： 所有以02开头的SQLSTATE代码的简写
  	SQLEXCEPTION：所有没有被SQLWARNING或NOT FOUND捕获的SQLSTATE代码的简写
  ```

- 示例 读取年龄小于输入年龄的员工姓名和职位并插入到新的表中

  ```
  create procedure p11(in uage int)
  begin
  	declare uname varchar(100);
  	declare upro varchar(100);
  	declare u_cursor cursor for select name,profession from tb_user where age <= uage;
  	decaler exit handler for SQLSTATE '02000' close u_cursor;
  	
  	drop table if exists tb_user_pro;
  	
  	create table if not exists tb_user_pro(
  		id int primary key auto increment;
  		name varchar(100);
  		profession varchar(100);
  	);
  	
  	open u_cursor;
  	while true do
  		fetch u_cursor into uname, upro;
  		insert into tb_user_pro values (null, uname, upro);
  	end while;
  	
  end;
  
  call p11();
  ```

  decaler exit handler for SQLSTATE '02000' close u_cursor; 也可以写为decaler exit handler for not found close u_cursor;

### 存储函数

存储函数是有返回值的存储过程，存储函数的参数只能是in类型的。 具体语法如下：

```
CREATE FUNCTION 存储函数名称([参数列表])
RETURNS type [characteristic...]
BEGIN
	SQL语句
	RETURN ...;
END;

```

characteristic 说明：

- DETERMINISTIC: 相同的输入参数总是产生相同的结果
- NO SQL : 不包含SQL语句
- READS SQL DATA: 包含读取数据的语句，但不包含写入数据的语句。

```mysql
---1到n累加

create function func1(n int)
returns int deterministic

begin
	declare total int default 0;
	
	while n > 0 do
		set total := total + n;
		set n := n - 1;
	end while;
	
	return total;
end;

select func1(50);

```

### 触发器

触发器是与表有关的数据库对象，是指在insert、update、delete之前或之后，触发并执行触发器中定义的SQL语句集合。触发器的这种特性可以协助应用在数据库端确保数据的完整性， 日志记录， 数据校验等操作。

使用别名OLD和NEW来引用触发器中发生变化的内容，这与其他数据库是相似的，现在触发器还支持行级触发，不支持语句级触发。

```
触发器类型    NEW 和 OLD
insert型    NEW表示将要或者已经新增的数据
update型    OLD表示修改之前的数据，NEW表示将要或已经修改之后的数据
delete型    OLD表示将要或者已经删除的数据
```

- 创建

  ```mysql
  CREATE TRIGGER trigger_name
  BEFORE/ALTER INSERT/UPDATE/DELETE
  ON table_name FOR EACH ROW
  BEGIN
  	trigger_stmt;
  END;
  ```

  

- 查看

  ```
  SHOW TRIGGERS;
  ```

  

- 删除

  ```mysql
  DROP TRIGGER [schema_name.]trigger_name; -- 如果没有指定schema_name, 默认为当前数据库。
  ```

- 练习

  通过触发器记录tb_user表的数据变更日志，将变更日志插入到日志表user_logs中，包含增加、修改、删除；

  ```mysql
  create table user_logs(
  	id int(11) not null auto_increament,
  	operation varchar(20) not nulkl comment '操作类型，insert/update/delete',
  	operte_time datetime not null comment '操作时间',
  	operate_id int(11) not null comment '操作的ID',
  	operate_params varchar(500) comment '操作的参数'，
  	primary key('id')
  ) engine=innodb default charset=utf8;
  
  -- 插入触发器
  create trigger tb_user_insert_trigger
  	after insert on tb_user for each row
  begin
  	insert into user_logs(id, operation, operate_time, operate_id, operate_param) values(null, 'insert', now(), new.id, concat('插入的数据内容为：id=', new,id,'name=', new.name, ....));
  end
  ```

  

## 锁

### 概述

​		锁是计算机协调多个进程或线程并发访问某一资源的机制。在数据库中，除传统的计算机资源CPU、RAM、I/O的争用以外，数据也是一种供许多用户共享的资源。如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。

### 分类

按照锁的粒度，分为三类：

1. 全局锁： 锁定数据库中的所有表
2. 表级锁： 锁住整张表
3. 行级锁：锁住对应的行数据

### 全局锁

​		全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML语句，DDL语句，已经更新操作的事务提交语句都将被阻塞。

其典型的使用场景是做全库的逻辑备份，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性。

- 演示

  ```mysql
  flush tables with read lock;   -- 加全局锁
  -- 在windows命令行下执行
  mysqldump -uroot -p1234 数据库名 > 数据库名.sql  -- 备份
  
  -- 备份期间不能执行 DML DDL，只能执行DQL语句
  --备份完成后释放锁
  unlock tables;
  ```

- 特点

  数据库中加全局锁是一个比较重的操作，存在以下问题：

  - 如果在主库上备份， 那么备份期间不能执行更新，业务基本上就得停摆
  - 如果在从库上备份，那么在备份期间从库不能进行主库同步过来的二进制日志，会导致主从延迟

  在lnnodb引擎中，可以在备份时加上参数 --single-transcation参数来完成不加锁的一致性数据备份

  ```mysql
  mysqldump --single-transaction -uroot -p1234 itcast>itcast.sql
  ```

### 表级锁

表级锁每次操作锁住整张表。锁定粒度大，发生锁冲突的概率最高，并发度最低。应用在MyISAM、Innodb、DBD存储引擎中。

表级锁主要分为以下三类：

- 表锁
- 元数据锁
- 意向锁

#### 表锁

分为两类：

- 表共享读锁 read lock

  加了读锁之后，当前客户端和其他客户端都可以读，写会阻塞

- 表独占写锁 write lock

  当前客户端加锁之后可读可写，其他客户端进行读写操作将阻塞

语法：

1. 加锁 lock tables 表名... read/write
2. 释放锁 unlock tables / 客户端连接断开

#### 源数据锁 meta data lock MDL

MDL加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。MDL锁主要作用是维护表元数据的数据一致性，在表上有活动事务的时候，不可以对元数据进行写入操作。为了避免DML与DDL冲突，保证读写的正确性。即在一个客户端中进行DML操作的事务时，另一个客户端中不能执行alter等DDL语句。



在mysql5.5中引入了MDL，当对一张表进行增删改的时候，加MDL读锁(共享)；当对表结构进行变更操作的时候，加MDL写锁（排他）。

```
对应SQL                           锁类型                              说明
lock tables xxx read/write    shared_read_only/shared_no_read_write
select\select ...lock in share mode shared_read      与shared_read、shared_write兼容，与exclusive互斥
insert\update\delete\select...for update shared_write 与shared_read、shared_write兼容，与exclusive互斥
alter table...               exclusive                与其他的MDL都互斥
```



查看元数据锁

```mysql
select object_type, object_schema, object_name, lock_type, lock_duration from performance_schema.metadata_locks;
```

#### 意向锁

为了避免DML在执行时，加的行锁与表锁的冲突，在innodb中引入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查。其解决的问题就是行锁和表锁的冲突问题。

1. 意向共享锁IS：由语句select...lock in share mode添加
2. 意向排他锁IX：由insert、update、delete、select...for update添加

意向锁和表锁的兼容性

1. 意向共享锁IS：与表锁共享锁read兼容，与表锁排他锁write互斥
2. 意向排他锁IX：与表锁共享锁read及排它锁write都互斥。意向锁之间不会互斥。

可以通过一下sql查看意向锁及行锁的加锁情况

```mysql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
```

### 行级锁

行级锁，每次操作锁住对应的行数据。锁定粒度最小，发生锁冲突的概率最低，并发度最高。应用在innodb存储引擎中。

innodb的数据是基于索引组织的，行锁是通过对索引上的索引项加锁来实现的，而不是对记录加的锁。对于行级锁，主要分为以下三类：

- 行锁record lock：锁定单个记录的锁，防止其他事务对此行进行update和delete。在RC、RR隔离级别下都支持。
- 间隙锁：锁定索引记录间隙（不包含该记录），确保索引记录间隙不变，防止其他事务在这个间隙进行insert，产生幻读。在RR隔离级别下都支持。
- 邻键锁next-key lock：行锁和间隙锁的组合，同时锁住数据，并锁住数据前面的间隙Gap。在RR隔离级别下支持。

#### 行锁

1. 共享锁S：允许一个事务去读一行，阻止其他事务获得相同数据集的排他锁。即共享锁之间兼容，与排他锁互斥
2. 排他锁X：允许获取到排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁

| SQL                          | 行锁类型   | 说明                                     |
| ---------------------------- | ---------- | ---------------------------------------- |
| insert                       | 排他锁     | 自动加锁                                 |
| update                       | 排他锁     | 自动加锁                                 |
| delete                       | 排他锁     | 自动加锁                                 |
| select...                    | 不加任何锁 |                                          |
| select... lock in share mode | 共享锁     | 需要手动在select之后加lock in share mode |
| select ... for update        | 排他锁     | 需要手动在select之后加for update         |

- 演示

  默认情况下，innodb在repeatable read事务隔离级别运行，innodb使用next-key 锁进行搜索和索引扫描，以防止幻读。

  1. 针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁
  2. innodb的行锁是针对索引加的锁，不通过索引条件检索数据，那么innodb将对表中所有记录加锁，此时就会升级为表锁。

  可以通过以下SQL查看意向锁及行锁的加锁情况

  ```mysql
  select object_schema, object_name,index_name,lock_type,lock_mode,lock_data from performance_schema.data_locks;
  ```

#### 间隙锁 临键锁

默认情况下，innodb在repeatable read事务隔离级别运行，innodb使用next-key 锁进行搜索和索引扫描，以防止幻读。

1. 索引上的等值查询（唯一索引）给不存在的记录加锁时，优化为间隙锁
2. 索引上的等值查询（普通查询）向右遍历时最后一个值不满足查询需求时， next-key lock退化为间隙锁
3. 索引上的范围查询（唯一查询）会访问到不满足条件的第一个值为止，也会加上临键锁
4. 注意：间隙锁的唯一目的是防止其他事务插入间隙。间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。



## Innodb引擎

### 逻辑存储结构

表空间tablespace，ibd文件，一个mysql实例可以对应多个表空间，用于存储记录索引等数据。表空间中有多个段

段 segment，分为数据段、索引段、回滚段，innodb是索引组织表，数据段就是B+树的叶子节点，索引段即为B+树的非叶子节点。段用来管理多个区。

区extend，表空间的单元结构，每个区的大小为1M。默认情况下，Innodb存储引擎页大小为16k，即一个区中一共有64个连续的页。

页page，是innodb存储引擎磁盘管理的最小单元，每个页的大小默认为16kB。为了保证页的连续性，innodb引擎每次从磁盘申请4-5个区。

行，innodb存储引擎数据是按行进行存放的

### 架构

mysql5.5版本开始，默认使用Innodb存储引擎。他擅长事务处理，具有崩溃恢复特性， 日常开发中非常广泛。

#### 内存架构

- buffer pool：缓冲池是主内存中的一个区域，里面可以缓存磁盘上经常操作的真实数据，在执行增删改查操作时， 先操作缓冲池中的数据（若缓冲池中没有数据，则从磁盘加载并缓存），然后再以一定频率刷新到磁盘，从而减少磁盘IO， 加快处理速度。

  缓冲池以page页为单位，底层采用连表数据结构管理Page。根据状态，将page分为三种类型：

  - free page: 空闲page， 未被使用
  - clean page：被使用page，数据没有被修改过
  - dirty page：脏页，被使用page，数据被修改过，其中数据与磁盘的数据产生了不一致

- change buffer：更改缓冲区（针对非唯一的二级索引页），执行dml语句时，如果这些数据page没有在buffer pool中，不会直接操作磁盘，而是将数据变更存在更改缓冲区中，在未来数据被读取时，再将数据合并恢复到buffer pool中，再将合并后的数据刷新到磁盘中。

  change buffer的意义：

  与聚集索引不同，二级索引通常是非唯一的，并且以相对随机的顺序插入二级索引。同样，删除和更新可能会影响索引树中不相邻的二级索引页，如果每一次都操作磁盘，会造成大量的磁盘IO。有了change buffer之后，我们可以在缓冲池中进行合并处理，减少磁盘IO。

- adaptive_hash_index 自适应hash索引，用于优化对buffer pool数据的查询。innodb 存储引擎会监控对表上各索引页的查询，如果观察到hash索引可以提升速度，则建立hash索引，称之为自适应hash索引。

  自适应hash索引，无需人工干预，是系统根据情况自动完成。

  参数 adaptive_hash_index

- log buffer 日志缓冲区，用来保存要写入磁盘中的log日志数据（redo log， undo log）， 默认大小为16MB，日志缓冲区的日志会定期刷新到磁盘中。如果需要更新、插入或者删除许多行的事务，增加日志缓冲区的大小可以节省磁盘IO

  参数：

  - innodb_log_buffer_size：缓冲区大小
  - innodb_flush_log_at_trx_commit: 日志刷新到磁盘的时机
    - 1： 日志在每次事务提交时写入并刷新到地盘
    - 0： 每秒将日志写入并刷新到磁盘一次
    - 2： 日志在每次事务提交后写入，并每秒刷新到磁盘一次

#### 磁盘结构

- system tablespace 系统表空间是更改缓冲区的存储区域。如果表是在系统空间而不是每个文件或者通用表空间创建的，他也可能包含表和索引结构。在mysql5.x还包含innodb数据字典、undolog等

  参数：innodb_data_file_path

- file-per-table tablespace: 每个表的文件表空间包含单个innodb表的数据和索引，并存储在文件系统的单个数据文件中

  参数：innodb_file_per_table

- Generate tablespace: 通用表空间，需要通过create tablespace 语法创建通用表空间，在创建表时，可以指定该表空间

  ```mysql
  create tablespace xxxx add datafile 'filename' egine=engine_name;
  ```

  

- Undo tablespaces撤销表空间，MySQL实例在初始化时会自动创建两个默认的undo表空间，初始化大小16M，用于存储undo log日志。

- Temporary tablespaces Innodb使用会话临时表空间和全局临时表空间。存储用户创建的临时表等数据。

- doublewrite buffer files 双写缓冲区，innodb引擎将数据页从buffer pool刷新到磁盘前，先降数据写入双写缓冲区，便于系统异常时恢复数据

- redo log 重做日志，是用来实现事务的持久性。该日志文件又两部分组成：重做日志缓冲redo  log buffer以及重做日志文件redo log。前者是在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都会存到该日志中，用于在刷新脏页到磁盘时，发生错误时，进行数据恢复使用。

  以循环方式写入重做日志文件，涉及两个文件 ib_logfile0 ib_logfile1

#### 后台线程

- master thread 核心后台线程，负责调度其他线程，还负责将缓冲池中的数据异步刷新到磁盘中，保持数据的一致性，还包括脏页的刷新、合并插入缓存、undo页的回收

- IO  thread 在innodb存储引擎中大量使用了AIO来处理IO请求，这样可以极大地提高数据库的性能，而IO thread主要负责这些IO请求的回调

  | 线程类型             | 默认个数 | 职责                         |
  | -------------------- | -------- | ---------------------------- |
  | read thread          | 4        | 负责读操作                   |
  | write thread         | 4        | 负责写操作                   |
  | log thread           | 1        | 负责将日志缓冲区刷新到磁盘   |
  | insert buffer thread | 1        | 负责将写缓冲区内容刷新到磁盘 |

  show engine innodb status；可以查看存储引擎状态信息

- purge thread 主要用于回收事务已经提交了的undo log， 在事务提交之后，undo log可能不用了，就用他来回收。

- page cleaner thread 协助master thread刷新脏页到磁盘的线程，他可以减轻master thread的工作压力，减少阻塞。

### 事务原理

#### 事务

事务是一组操作的集合，它是不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

特性：

原子性

一致性

隔离性

持久性



原子性、一致性、持久性是redo log和undo log来保证的

隔离性是锁和mvcc保证的

#### redo log     事务的持久性

重做日志，记录的事务提交时数据页的物理修改，是用来实现事务的持久性

该日志文件由两部分组成：重做日志缓冲区redo log buffer以及重做日志文件redo log file，前者在内存中，后者在磁盘中。当事务提交之后会把所有修改信息都存到改日志文件中，用于在刷新脏页到磁盘，发生错误时，进行数据恢复使用。

#### undo log   事务的原子性

回滚日志，用于记录数据被修改前的信息，作用包含两个：提供回滚和MVCC（多版本并发控制）

undo log 和redo log记录物理日志不一样，他是逻辑日志。可以认为当delete一条记录时， undo log会记录一条对应的insert记录，反之亦然，当update一条记录时，他记录一条对应相反的update记录。当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。

undo log销毁：undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志可能还用于MVCC

undo log存储：undo log采用段的方式进行管理和记录，存放在前面介绍的rollback segment回滚段中，内部包含1024个undo log segment。



### MVCC

#### 基本概念

- 当前读

  读取的是记录的最新版本，读取时还要保证其他并发事务不能修改当前记录，会对读取的记录进行加锁。对于我们日常的操作，如：select ...lock in share mode(共享锁)，select...for update\update\insert\delete（排他锁）都是一种当前读

- 快照读

  简单的select不加锁就是快照读，快照读，读取的是记录数据的可见版本，有可能是历史数据，不加锁，是非阻塞读

  - read committed： 每次select，都生成一个快照读
  - repeatable read：开启事务后第一个select语句才是快照读的地方
  - serializable：快照读会退化为当前读

- MVCC

  全称Multi-Version Concurrency Control，多版本并发控制。指维护一个数据的多个版本，使得读写操作没有冲突，快照读为MySQL实现mvcc提供了一个非阻塞读功能。mvcc的具体实现，还需要依赖于数据库记录中的三个隐式字段、undo log日志、readView。

#### 隐藏字段

- 记录中的隐藏字段

  

| 隐藏字段    | 含义                                                         |
| ----------- | ------------------------------------------------------------ |
| DB_TRX_ID   | 最近修改事务ID，记录插入这条记录或最后一次修改该记录的事务ID |
| DB_ROLL_PTR | 回滚指针，指向这条记录的上一个版本，用于配合undo log，指向上一个版本 |
| DB_ROLL_ID  | 隐藏主键，如果表结构没有指定主键，将会生成改隐藏字段         |

ibd2sdi 表名.ibd  查看三个隐藏字段

#### undo log日志

回滚日志，在insert、update、delete的时候产生的便于数据回滚的日志。

当insert的时候，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除。

而update、delete的时候，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会立即被删除

##### undo log版本链

不同事务或相同事务对同一条记录进行修改，会导致改记录的undolog生成一条记录版本链表，链表的头部是最新的旧记录，链表的尾部是最早的旧记录。

#### readview

readview(读视图)是快照读，SQL执行时mvcc提取数据的依据，记录并维护系统当前活跃的事务（未提交的）id。

readview包含了四个核心字段：

| 字段           | 含义                           |
| -------------- | ------------------------------ |
| m_ids          | 当前活跃的事务ID集合           |
| min_trx_id     | 最小活跃事务id                 |
| max_trx_id     | 预分配事务id，当前最大事务id+1 |
| creator_trx_id | readview 创建者的事务id        |

版本链数据访问规则：trx_id代表当前事务id

- trx_id == creator_trx_id? 可以访问该版本。成立，说明数据是当前这个事务更改的
- trx_id < min_trx_id?可以访问该版本。成立，说明数据已经提交了
- trx_id>max_trx_id?不可以访问该版本，说明该事务在readview生成后才开启
- min_trx_id<=trx_id<=max_trx_id?如果trx_id不在m_ids中是可以访问该版本的。成立，说明数据已经提交

不同的隔离级别，生成readview的时机不同：

- read commited： 在事务中每一次执行快照读时生成readview
- repeatable read：仅在事务中第一次执行快照读时生成readview，后续复用该readview



## MySQL管理

### 系统数据库

MySQL安装完成后自带了以下四个数据库

| 数据库             | 含义                                                         |
| ------------------ | ------------------------------------------------------------ |
| mysql              | 存储服务器正常运行所需的各种信息（时区、主从、用户、权限）   |
| information_schema | 提供了访问数据库元数据的各种表和视图，包含数据库、表、字段类型以及访问权限等 |
| performance_schema | 为MySQL服务器运行时状态提供了一个底层监控功能，主要用于手机数据库服务器性能参数 |
| sys                | 包含了一系列方便DBA和开发人员利用performance_schema性能数据进行性能调优和诊断的视图 |

### 常用工具

- mysql 客户端

  mysql [options] [databases]

  选项：

  	- -u  ，--user=name  指定用户名
  	- -p  ，--password=[] 指定密码
  	- -h                        指定服务器ip
  	- -P                      指定连接端口
  	- -e                 执行SQL语句并退出

  ```
  mysql -uroot -p1234 db01 -e "select * from stu;"
  ```

- mysqladmin

  执行管理操作的客户端程序。检查服务器的配置和当前状态、创建并删除数据库等。

- mysqlbinlog

  由于服务器生成的二进制文件以二进制格式保存，所以如果想要检查这些文本的文本格式，就会使用到mysqlbinlog日志管理工具

- mysqlshow

  客户端对象查找工具，用来很快地查找存在哪些数据库、数据库中的表、表中的列和索引

- mysqldump

  客户端工具用来备份数据库或者在不同数据库之间进行数据迁移。备份内容包含创建表，及插入表的SQL语句。

- mysqlimport / source

  mysqlimport 是客户端数据导入工具，用来导入mysqldump -T参数导出的文本文件

  如果需要导入sql文件可以使用source xxx.sql指令

# 运维篇

## 日志

### 错误日志

错误日志是MySQL中最重要的日志之一，他记录了当mysqld启动和停止时，以及服务器在运行过程中发生的任何严重错误时的相关信息。当数据库出现任何故障导致无法正常使用时，建议首先查看此日志。

该日志是默认开启的，默认存放目录/var/log/，默认的日志文件名为mysqld.log。查看日志位置：

```mysql
show variables like '%log_error%';
```



### 二进制日志

二进制日志记录了所有的DDL和DML语句，但不包括数据查询（select，show）语句

作用：

- 灾难时的数据恢复
- mysql的主从复制
- show variables like '%log_bin%'

日志格式

| 日志格式  | 含义                                                         |
| --------- | ------------------------------------------------------------ |
| statement | 基于sql语句的日志记录，记录的是sql语句，对数据进行修改的sql都会记录在日志文件中 |
| row       | 基于行的日志记录，记录的是每一行的数据变更                   |
| mixed     | 混合了statement和row两种格式，默认采用statement，在某些特殊情况下自动切换为row进行记录 |

show variables like '%binglog_format%';

- 日志查看

  由于日志是以二进制方式存储的，不能直接读取，需要通过二进制日志查询工具mysqlbinlog来查看

  ```
  mysqlbinlog [参数选项] logfilename
  参数选项：
  	-d 指定数据库名称，只列出指定的数据库相关操作
  	-o 忽略掉日志中的前n行命令
  	-v 将行事件（数据变更）重构为sql语句
  	-vv 将行事件（数据变更）重构为sql语句，并输出注释信息
  ```

- 日志删除

  对于比较繁忙的业务系统，每天生成的binlog数据巨大，如果长时间不清除，将会占用大量磁盘空间。可以通过以下几种方式清理日志。

  | 指令                                             | 含义                                                         |
  | ------------------------------------------------ | ------------------------------------------------------------ |
  | reset master                                     | 删除全部binlog日志，删除之后，日志编号将从binlog.000001重新开始 |
  | purge master logs to ‘binlog.********’           | 删除*编号之前的所有日志                                      |
  | purge master logs before ‘yyyy-mm-dd hh24:mi:ss’ | 删除日志为指定日期之前的产生的所有日志                       |

  也可以在mysql的配置文件中设置二进制日志的过期时间，设置了之后， 二进制日志过期之后会自动删除。

  show variables like '%binlog_expire_logs_seconds%'

### 查询日志

查询日志中记录了客户端的所有操作语句，而二进制日志不包含查询数据库的sql语句。默认情况下，查询日志是未开启的。如果设置开启查询日志，show variables like '%general%'

修改MySQL的配置文件/etc/my.cnf文件，添加如下内容 

开启查询日志 0关闭 1开启

general_log = 1 

设置日志的文件名，如果没有指定，默认文件名为host_name.log

general_log_file=mysql_query,log

### 慢查询日志

记录了所有执行时间超过参数long_query_time设置值并且扫描记录数小于min_examined_row_limit的所有的SQL语句的日志，默认未开启。long_query_time默认为10秒，最小为0，精度可以到微秒。也是要修改mysql配置文件生效

```mysql
# 慢查询日志
slow_query_log=1
# 执行时间参数, 以下设置为执行时间超过2s写入慢查询日志localhost-slow.log
long_query_time = 2
```

默认情况下，不会记录管理语句，也不会记录不使用索引进行查找的查询。可以使用log_slow_admin_statements和更改此行为log_queries_not_using_indexes，如下

```mysql
# 记录执行较慢的管理语句
log_slow_admin_statements = 1
# 记录执行较慢的未使用索引的语句
log_queries_not_using_indexes = 1
```



## 主从复制

## 分库分表

## 读写分离

