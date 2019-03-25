# 约束

- NOT NULL 约束：保证列中数据不能有 NULL 值
- DEFAULT 约束：提供该列数据未指定时所采用的默认值
- UNIQUE 约束：保证列中的所有数据各不相同
- 主键约束：唯一标识数据表中的行/记录
- 外键约束：唯一标识其他表中的一条行/记录
- CHECK 约束：此约束保证列中的所有值满足某一条件
- 索引：用于在数据库中快速创建或检索数据

## `NOT NULL`约束
给某字段添加`NOT NULL`约束
```sql
ALTER TABLE CUSTOMERS
	MODIFY SALARY DECIMAL (18, 2) NOT NULL;
```
## `DEFAULT`约束
给某字段添加`DEFAULT`约束
```sql
ALTER TABLE CUSTOMERS
	MODIFY SALARY DECIMAL (18, 2) DEFAULT 5000.00;
```
----------
删除某字段`DEFAULT`约束
```sql
ALTER TABLE CUSTOMERS
	ALTER COLUMN SALARY DROP DEFAULT;
```
## `UNIQUE`约束
给某字段添加`UNIQUE`约束
```sql
ALTER TABLE CUSTOMERS
	MODIFY AGE INT NOT NULL UNIQUE;
```
----------
将多个列定义`UNIQUE`约束
```sql
ALTER TABLE CUSTOMERS
	ADD CONSTRAINT myUniqueConstraint UNIQUE(AGE, SALARY);
```
----------
删除`UNIQUE`约束
```sql
ALTER TABLE CUSTOMERS
	DROP CONSTRAINT myUniqueConstraint;
```

## 主键(Primary Key)约束
将某个字段定义为主键
```sql
ALTER TABLE CUSTOMERS
	ADD PRIMARY (ID);
```

----------
将多个字段定义为主键(组合键)
```sql
ALTER TABLE CUSTOMERS
	ADD CONSTRAINT PK_CUSTID PRIMARY KEY (ID, NAME);
```

----------
删除主键
```sql
ALTER TABLE CUSTOMERS DROP PRIMARY KEY;
```

## 外键(FOREIGN KEY)约束
设置外键
`CUSTOMERS` 表：
```sql
CREATE TABLE CUSTOMERS(
	ID INT NOT NULL,
	NAME VARCHAR (20) NOT NULL,
	AGE INT NOT NULL,
	ADDRESS CHAR (25) ,
	SALARY DECIMAL (18, 2),
	PRIMARY KEY (ID)
);
```
`ORDERS` 表：
```sql
CREATE TABLE ORDERS (
	ID INT NOT NULL,
	DATE DATETIME,
	CUSTOMER_ID INT references CUSTOMERS(ID),
	AMOUNT double,
	PRIMARY KEY (ID)
);
```
如果 ORDERS 表已经存在，并且没有设置外键，那么可以使用下面的语法来修改数据表以指定外键
```sql
ALTER TABLE ORDERS
	ADD FOREIGN KEY (Customer_ID) REFERENCES CUSTOMERS (ID);
```

----------
删除外键约束
```sql
ALTER TABLE ORDERS
	DROP FOREIGN KEY;
```

## `CHECK`约束
给某字段添加`CHECK`约束
```sql
ALTER TABLE CUSTOMERS
	MODIFY AGE INT NOT NULL CHECK (AGE >= 18 );

// 或使用以下语法
ALTER TABLE CUSTOMERS
	ADD CONSTRAINT myCheckConstraint CHECK(AGE >= 18);
```

----------
删除`CHECK`约束
```sql
ALTER TABLE CUSTOMERS
	DROP CONSTRAINT myCheckConstraint;
```
## 索引约束
创建索引
```sql
CREATE INDEX index_name
	ON table_name ( column1, column2.....);
```

----------
删除索引
```sql
ALTER TABLE table_name
	DROP INDEX index_name;
```

## 完整性约束
完整性约束用于保证关系型数据库中数据的精确性和一致性。对于关系型数据库来说，数据完整性由参照完整性（referential integrity，RI）来保证。
有很多种约束可以起到参照完整性的作用，这些约束包括主键约束（Primary Key）、外键约束（Foreign Key）、唯一性约束（Unique Constraint）以及上面提到的其他约束。

# 数据完整性
- 实体完整性：表中没有重复的行
- 域完整性：通过限制数据类型、格式或者范围来保证给定列的数据有效性
- 参照完整性：不能删除被其他记录引用的行
- 用户定义完整性：施加某些不属于上述三种完整性的业务规则

## 创建数据库
在 RDBMS 中，数据库名应该是唯一的。
```sql
CREATE DATABASE DatabaseName;
```

## 删除数据库
```sql
DROP DATABASE DatabaseName;
```

## 选择数据库
如果你的数据库架构中有多个数据库同时存在，那么在开始操作之前必须先选定其中一个。
``
USE DatabaseName;
``

## 显示现有数据库列表
```sql
SHOW DATABASES;
```
# 语法

## 创建表
创建一个基本的表需要做的工作包括：命名表、定义列和各列的数据类型。
```sql
CREATE TABLE table_name(
	column1 datatype,
	column2 datatype,
	column3 datatype,
	.....
	columnN datatype,
	PRIMARY KEY( one or more columns )
);
```

## 删除表
SQL DROP TABLE 语句用于移除表定义以及表中所有的数据、索引、触发器、约束和权限设置。
```sql
DROP TABLE table_name;
```

## INSERT 语句
SQL INSERT INTO 语句用于向数据库中的表添加新行。
```sql
// 第一种语法格式
INSERT INTO TABLE_NAME (column1, column2, column3,...columnN)
VALUES (value1, value2, value3,...valueN);
```
如果要为表中所有的字段都添加数据的话，就不需要指定字段名了。不过这种情况下，必须保证值的顺序按照表中字段出现的顺序排列。
```sql
// 第二种语法格式
INSERT INTO TABLE_NAME VALUES (value1,value2,value3,...valueN);
```

## SELECT 语句
SQL SELECT 语句用于从数据库的表中取回所需的数据，并以表的形式返回。返回的表被称作结果集。
``` sql
SELECT column1, column2, columnN FROM table_name;
```
这里，column1, column2...是你想要从表中取回的字段。如果要取回表中所有字段的话，可以使用下面的语法：
```sql
SELECT * FROM table_name;
```

## UPDATE 语句
SQL UPDATE 语句用于修改表中现有的记录。
```sql
UPDATE table_name
SET column1 = value1, column2 = value2...., columnN = valueN
WHERE [condition];
```

## DELETE 语句
SQL DELETE 语句用于删除表中现有的记录。
```sql
DELETE FROM table_name
WHERE [condition];
```

## LIKE 子句
SQL LIKE 子句通过通配符来将一个值同其他相似的值作比较。可以同 LIKE 运算符一起使用的通配符有两个：
1. 百分号（%）:代表零个、一个或者多个字符
2. 下划线（_）:代表单个数字或者字符

示例：

| 语句                      | 描述                                     |
| ------------------------- | :--------------------------------------- |
| WHERE SALARY LIKE '200%'  | 找出所有 200 开头的值                    |
| WHERE SALARY LIKE '%200%' | 找出所有含有 200 的值                    |
| WHERE SALARY LIKE '_00%'  | 找出所有第二位和第三位为 0 的值          |
| WHERE SALARY LIKE '2_%_%' | 找出所有以 2 开始，并且长度至少为 3 的值 |
| WHERE SALARY LIKE '%2'    | 找出所有以 2 结尾的值                    |
| WHERE SALARY LIKE '_2%3'  | 找出所有第二位为 2，并且以3结束的值      |
| WHERE SALARY LIKE '2___3' | 找出所有以 2 开头以 3 结束的五位数       |

## TOP、LIMIT 和 ROWNUM 子句
这三个子句都是用于从一张数据表取回前N个或者X% 的记录。针对不同的数据库系统，使用不同的子句。Sql Server支持TOP子句，MySQL支持LIMIT子句，Oracle支持ROWNUM子句。
```sql
// TOP子句
SELECT TOP number|percent column_name(s)
	FROM table_name
	WHERE [condition]
// LIMIT子句
SELECT * FROM CUSTOMERS
	LIMIT 3;
// ROWNUM子句
SELECT * FROM CUSTOMERS
	WHERE ROWNUM <= 3;
```

## ORDER BY 子句
SQL ORDER BY 子句根据一列或者多列的值，按照升序或者降序排列数据。
```sql
SELECT column-list
	FROM table_name
	[WHERE condition]
	[ORDER BY column1, column2, .. columnN] [ASC | DESC];
```

## GROUP BY 子句
SQL GROUP BY 子句与 SELECT 语句结合在一起使用，可以将相同数据分成一组。
在 SELECT 语句中，GROUP BY 子句紧随 WHERE 子句，在 ORDER BY 子句之前(如果有)。
```sql
SELECT column1, column2
	FROM table_name
	WHERE [ conditions ]
	GROUP BY column1, column2
	ORDER BY column1, column2
```
实例：
```sql
id  name  dept  salary  edlevel  hiredate   
1 张三 开发部 2000 3 2009-10-11  
2 李四 开发部 2500 3 2009-10-01  
3 王五 设计部 2600 5 2010-10-02  
4 王六 设计部 2300 4 2010-10-03  
5 马七 设计部 2100 4 2010-10-06  
6 赵八 销售部 3000 5 2010-10-05  
7 钱九 销售部 3100 7 2010-10-07  
8 孙十 销售部 3500 7 2010-10-06 
```
sql语句：
```sql
SELECT dept, MAX(salary) AS MAXIMUM FROM STAFF GROUP BY dept
```
查询结果如下：
```sql
dept  MAXIMUM   
开发部 2500  
设计部 2600  
销售部 3500 
```
分析：使用`GROUP BY`对部门进行分组，然后对每个部门返回一个结果(一行记录)，即每个部门的最高薪水。


## DISTINCT 关键字
```sql
SELECT DISTINCT column1, column2,.....columnN
	FROM table_name
	WHERE [condition]
```

# SQL连接类型
SQL 中有多种不同的连接：
- 内连接（INNER JOIN）：当两个表中都存在匹配时，才返回行。
- 左连接（LEFT JOIN）：返回左表中的所有行，即使右表中没有匹配的行。
- 右连接（RIGHT JOIN）：返回右表中的所有行，即使左表中没有匹配的行。
- 全连接（FULL JOIN）：只要某一个表存在匹配，就返回行。
- 笛卡尔连接（CARTESIAN JOIN）：返回两个或者更多的表中记录集的笛卡尔积。

## 内连接
```sql
SELECT table1.column1, table2.column2...
FROM table1
INNER JOIN table2
ON table1.common_field = table2.common_field;
```

## 左连接
```sql
SELECT table1.column1, table2.column2...
FROM table1
LEFT JOIN table2
ON table1.common_field = table2.common_field;
```

## 右连接
```sql
SELECT table1.column1, table2.column2...
FROM table1
RIGHT JOIN table2
ON table1.common_field = table2.common_field;
```

## 全连接
```sql
SELECT table1.column1, table2.column2...
FROM table1
FULL JOIN table2
ON table1.common_field = table2.common_field;
```

## 笛卡尔连接
```sql
SELECT table1.column1, table2.column2...
FROM table1, table2 [, table3 ]
```

# UNION 子句
SQL UNION 子句/运算符用于将两个或者更多的 SELECT 语句的运算结果组合起来。
在使用 UNION 的时候，每个 SELECT 语句必须有相同数量的选中列、相同数量的列表达式、相同的数据类型，并且它们出现的次序要一致，不过长度不一定要相同。
```sql
SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition1]

UNION

SELECT column1 [, column2 ]
FROM table1 [, table2 ]
[WHERE condition1]
```

# NULL 值
SQL 中，NULL 用于表示缺失的值。数据表中的 NULL 值表示该值所处的字段为空。NULL 值与 0 或者包含空白（spaces）的字段是不同的，更多的表示的是未知。

# 别名(Alias)
使用别名（Alias）可以对数据表或者列进行临时命名。
表别名的基本语法如下：
```sql
SELECT column1, column2....
	FROM table_name AS alias_name
	WHERE [condition];
```
列别名的基本语法如下：
```sql
SELECT column_name AS alias_name
	FROM table_name
	WHERE [condition];
```
> 如果为表分配了别名，那么 Transact-SQL语句中对该表的所有显式引用都必须使用别名，而不能使用表名。

# 索引
索引是一种特殊的查询表，可以被数据库搜索引擎用来加速数据的检索。简单说来，索引就是指向表中数据的指针。数据库的索引同书籍后面的索引非常相像。
索引能够提高 SELECT 查询和 WHERE 子句的速度，但是却降低了包含 UPDATE 语句或 INSERT 语句的数据输入过程的速度。索引的创建与删除不会对表中的数据产生影响。
基本语法：

```sql
// 创建索引
CREATE INDEX index_name ON table_name;

// 删除索引
DROP INDEX index_name;
```
> 注意：删除索引时应当特别小心，数据库的性能可能会因此而降低或者提高。

## 单列索引
单列索引基于单一的字段创建，其基本语法如下所示：
```sql
CREATE INDEX index_name
	ON table_name (column_name);
```
## 唯一索引
唯一索引不止用于提升查询性能，还用于保证数据完整性。唯一索引不允许向表中插入任何重复值。其基本语法如下所示：
```sql
CREATE UNIQUE INDEX index_name
	ON table_name (column_name);
```
## 聚簇索引
聚簇索引在表中两个或更多的列的基础上建立。其基本语法如下所示：
```sql
CREATE INDEX index_name
	ON table_name (column1, column2);
```
> 说明：创建单列索引还是聚簇索引，要看每次查询中，哪些列在作为过滤条件的 WHERE 子句中最常出现。如果只需要一列，那么就应当创建单列索引。如果作为过滤条件的 WHERE 子句用到了两个或者更多的列，那么聚簇索引就是最好的选择。

## 隐式索引
隐式索引由数据库服务器在创建某些对象的时候自动生成。例如，对于主键约束和唯一约束，数据库服务器就会自动创建索引。

## 什么时候应当避免使用索引
尽管创建索引的目的是提升数据库的性能，但是还是有一些情况应当避免使用索引。
- 小的数据表不应当使用索引
- 需要频繁进行大批量的更新或者插入操作的表
- 如果列中包含大数或者 NULL 值，不宜创建索引
- 频繁操作的列不宜创建索引

# ALTER TABLE 命令
SQL ALTER TABLE 命令用于添加、删除或者更改现有数据表中的列。还可以用 ALTER TABLE 命令来添加或者删除现有数据表上的约束。
- 添加列：
```sql
ALTER TABLE table_name ADD column_name datatype;
```
- 删除列：
```sql
ALTER TABLE table_name DROP COLUMN column_name;
```
- 更改现有列的数据类型：
```sql
ALTER TABLE table_name MODIFY COLUMN column_name datatype;
```
- 添加NOT NULL约束：
```sql
ALTER TABLE table_name MODIFY column_name datatype NOT NULL;
```
- 添加唯一约束：
```sql
ALTER TABLE table_name
	ADD CONSTRAINT MyUniqueConstraint UNIQUE(column1, column2...);
```
- 添加CHECK约束：
```sql
ALTER TABLE table_name
	ADD CONSTRAINT MyUniqueConstraint CHECK (CONDITION);
```
- 添加主键约束：
```sql
ALTER TABLE table_name
	ADD CONSTRAINT MyPrimaryKey PRIMARY KEY (column1, column2...);
```
- 删除约束：
```sql
ALTER TABLE table_name
	DROP CONSTRAINT MyUniqueConstraint;
```
- 删除主键约束：
```sql
ALTER TABLE table_name
	DROP CONSTRAINT MyPrimaryKey;
```

# TRUNCATE TABLE 命令
SQL TRUNCATE TABLE 命令用于删除现有数据表中的所有数据。
与DROP TABLE 命令不同的是，DROP TABLE 命令不但会删除表中所有数据，还会将整个表结构从数据库中移除。如果想要重新向表中存储数据的话，必须重建该数据表。
基本语法：
```sql
TRUNCATE TABLE table_name;
```

# 视图
视图无非就是存储在数据库中并具有名字的 SQL 语句，或者说是以预定义的 SQL 查询的形式存在的数据表的成分。
视图可以包含表中的所有列，或者仅包含选定的列。视图可以创建自一个或者多个表，这取决于创建该视图的 SQL 语句的写法。
视图，一种虚拟的表，允许用户执行以下操作：
- 以用户或者某些类型的用户感觉自然或者直观的方式来组织数据；
- 限制对数据的访问，从而使得用户仅能够看到或者修改（某些情况下）他们需要的数据；
- 从多个表中汇总数据，以产生报表。

## 创建视图
CREATE VIEW 语句的基本语法如下所示：
```sql
CREATE VIEW view_name AS
	SELECT column1, column2.....
	FROM table_name
	WHERE [condition];
```
## 更新视图
视图可以在特定的情况下更新：
- SELECT 子句不能包含 DISTINCT 关键字
- SELECT 子句不能包含任何汇总函数（summary functions）
- SELECT 子句不能包含任何集合函数（set functions）
- SELECT 子句不能包含任何集合运算符（set operators）
- SELECT 子句不能包含 ORDER BY 子句
- FROM 子句中不能有多个数据表
- WHERE 子句不能包含子查询（subquery）
- 查询语句中不能有 GROUP BY 或者 HAVING
- 计算得出的列不能更新
- 视图必须包含原始数据表中所有的 NOT NULL 列，否则 INSERT 操作生效

如果视图满足以上所有的条件，该视图就可以被更新。最终更新的还是原始数据表，只是其结果反应在了视图上。
## 向视图中插入新行
可以向视图中插入新行，其规则同（使用 UPDATE 命令）更新视图所遵循的规则相同。
## 删除视图中的行
视图中的数据行可以被删除。删除数据行与更新视图和向视图中插入新行遵循相同的规则。
## 删除视图
基本语法:
```sql
DROP VIEW view_name;
```

# HAVING 子句
HAVING 子句使你能够指定过滤条件，从而控制查询结果中哪些组可以出现在最终结果里面。
> 与WHERE 子句的区别：WHERE 子句对被选择的列施加条件，而 HAVING 子句则对 GROUP BY 子句所产生的组施加条件。

HAVING 子句在 SELECT 查询中的位置：
```sql
SELECT column1, column2
FROM table1, table2
WHERE [ conditions ]
GROUP BY column1, column2
HAVING [ conditions ]
ORDER BY column1, column2
```

# 事务
事务是在数据库上按照一定的逻辑顺序执行的任务序列。事务实际上就是对数据库的一个或者多个更改。当你在某张表上创建更新或者删除记录的时，你就已经在使用事务了。控制事务以保证数据完整性，并对数据库错误做出处理，对数据库来说非常重要。
***事务是DBMS的基本单位*，是构成单一逻辑工作单元的操作集合**
## 事务的属性
事务具有以下四个标准属性，通常用缩略词 ACID 来表示：
- 原子性：保证任务中的所有操作都执行完毕；否则，事务会在出现错误时终止，并回滚之前所有操作到原始状态。
- 一致性：如果事务成功执行，则数据库的状态得到了进行了正确的转变。
- 隔离性：保证不同的事务相互独立、透明地执行。
- 持久性：即使出现系统故障，之前成功执行的事务的结果也会持久存在。

## 事务控制
有四个命令用于控制事务：
- COMMIT：提交更改；
- ROLLBACK：回滚更改；
- SAVEPOINT：在事务内部创建一系列可以 ROLLBACK 的还原点；
- SET TRANSACTION：命名事务；

### COMMIT 命令
COMMIT 命令用于保存事务对数据库所做的更改。COMMIT 命令会将自上次 COMMIT 命令或者 ROLLBACK 命令执行以来所有的事务都保存到数据库中。
基本语法：
```
COMMIT;
```

### ROLLBACK 命令
ROLLBACK 命令用于撤销尚未保存到数据库中的事务。ROLLBACK 命令只能撤销自上次 COMMIT 命令或者 ROLLBACK 命令执行以来的事务。
基本语法：
```
ROLLBACK;
```

### SAVEPOINT 命令
SAVEPOINT 是事务中的一个状态点，使得我们可以将事务回滚至特定的点，而不是将整个事务都撤销。
基本语法：
```
SAVEPOINT SAVEPOINT_NAME;
```
回滚至某一保存点的语法：
```
ROLLBACK TO SAVEPOINT_NAME;
```
删除先前创建的保存点：
```
RELEASE SAVEPOINT SAVEPOINT_NAME;
```
保存点一旦被释放，你就不能够再用 ROLLBACK 命令来撤销该保存点之后的事务了。

### SET TRANSACTION 命令
SET TRANSACTION 命令可以用来初始化数据库事务，指定随后的事务的各种特征。
例如，你可以将某个事务指定为只读或者读写。SET TRANSACTION 命令的语法如下所示：
```
SET TRANSACTION [ READ WRITE | READ ONLY ];
```

# 临时表
某些关系型数据库管理系统支持临时表。临时表是一项很棒的特性，能够让你像操作普通的 SQL 数据表一样，使用 SELECT、UPDATE 和 JOIN 等功能来存储或者操作中间结果。
临时表有时候对于保存临时数据非常有用。有关临时表你需要知道的最重要的一点是，它们会在当前的终端会话结束后被删除。
基本语法：
```sql
// 创建临时表
CREATE TEMPORARY TABLE SALESSUMMARY

// 删除临时表
DROP TABLE SALESSUMMARY;
```

# 子查询
子查询（Sub Query）或者说内查询（Inner Query），也可以称作嵌套查询（Nested Query），是一种嵌套在其他 SQL 查询的 WHERE 子句中的查询。
使用子查询必须遵循以下几个规则：
- 子查询必须括在圆括号中。
- 子查询的 SELECT 子句中只能有一个列，除非主查询中有多个列，用于与子查询选中的列相比较。
- 子查询不能使用 ORDER BY，不过主查询可以。在子查询中，GROUP BY 可以起到同 ORDER BY 相同的作用。
- 返回多行数据的子查询只能同多值操作符一起使用，比如 IN 操作符。
- SELECT 列表中不能包含任何对 BLOB、ARRAY、CLOB 或者 NCLOB 类型值的引用。
- 子查询不能直接用在集合函数中。
- BETWEEN 操作符不能同子查询一起使用，但是 BETWEEN 操作符可以用在子查询中。

## SELECT 语句中的子查询
```sql
SELECT * FROM CUSTOMERS
	WHERE ID IN (SELECT ID
	FROM CUSTOMERS
	WHERE SALARY > 4500) ;
```

## INSERT 语句中的子查询
```sql
// CUSTOMERS 表和CUSTOMERS_BKP 表拥有相似的结构
INSERT INTO CUSTOMERS_BKP
	SELECT * FROM CUSTOMERS
	WHERE ID IN (SELECT ID
	FROM CUSTOMERS) ;
```

## UPDATE 语句中的子查询
```sql
UPDATE CUSTOMERS
	SET SALARY = SALARY * 0.25
	WHERE AGE IN (SELECT AGE FROM CUSTOMERS_BKP
	WHERE AGE >= 27 );
```

## DELETE 语句中的子查询
```sql
DELETE FROM CUSTOMERS
	WHERE AGE IN (SELECT AGE FROM CUSTOMERS_BKP
	WHERE AGE > 27 );
```

# 使用序列
序列是根据需要产生的一组有序整数：1, 2, 3 ... 序列在数据库中经常用到，因为许多应用要求数据表中的的每一行都有一个唯一的值，序列为此提供了一种简单的方法。

## 使用 AUTO_INCREMENT 列
在 MySQL 中使用序列最简单的方式是，把某列定义为 AUTO_INCREMENT，然后将剩下的事情交由 MySQL 处理：
```sql
...
id INT UNSIGNED NOT NULL AUTO_INCREMENT
...
// 默认情况下，MySQL 中序列的起始值为 1,也可以设置为其他值
...
id INT UNSIGNED NOT NULL AUTO_INCREMENT = 100
...
```

# SQL 常用函数
|    函数     | 说明                               |
| :---------: | :--------------------------------- |
| COUNT 函数  | 用于统计返回的记录数               |
|  MAX 函数   | 用于找出记录集中具有最大值的记录   |
|  MIN 函数   | 用于找出记录集中具有最小值的记录   |
|  AVG 函数   | 用于找出表中记录在某字段处的平均值 |
|  SUM 函数   | 用于找出表中记录在某字段处的总和   |
|  SQRT 函数  | 用于计算得出任何数值的平方根       |
|  RAND 函数  | 用于产生 0 至 1 之间的随机数       |
| CONCAT 函数 | 用于将两个字符串连接为一个字符串   |

# 存储过程的好处
1、执行速度更快，在数据库中保存的存储过程语句都是编译过的
2、允许模块化程序设计，类似方法的复用
3、提高系统的安全性，防止SQL注入
4、减少网络流通量，只需要传输存储过程的名称

# tablespace
- 一个tablespace可以由多个datafile组成，一个datafile不能跨越多个tablespace 。
- table中的数据,通过hash算法分布在tablespace中的各个datafile中,tablespace是逻辑上的概念,datafile则在物理上储存了数据库的种种对象。
- 表空间是oracle数据库中最大的逻辑单位与存储空间单位，数据库系统通过表空间为数据库对象分配空间。表空间在物理上体现为磁盘数据文件，每一个表空间由一个或多个数据文件组成，一个数据文件只可与一个表空间相联系，这是逻辑与物理的统一。


# 常用语句
```sql
// 1.计算每个人的总成绩并排名(要求显示字段：姓名，总成绩)
select name, SUM(score) as allscore from stuscore group by name order by allscore

// 2.计算每个人的总成绩并排名(要求显示字段: 学号，姓名，总成绩)
select stuid, name, SUM(score) as allscore from stuscore group by stuid, name order by allscore

// 3.计算每个人单科的最高成绩(要求显示字段: 学号，姓名，课程，最高成绩)
select t1.stuid, t1.name, t1.subject, t1.score from stuscore t1,
(select stuid, MAX(score) as maxscore from stuscore group by stuid) t2
where t1.stuid = t2.stuid and t1.score = t2.maxscore order by t1.stuid

// 4.计算每个人的平均成绩（要求显示字段: 学号，姓名，平均成绩）
select distinct t1.stuid, t1.name, t2.avgscore from stuscore t1,
(select stuid, AVG(score) as avgscore from stuscore group by stuid) t2
where t1.stuid = t2.stuid order by t1.stuid

// 5.列出各门课程成绩最好的学生(要求显示字段: 学号，姓名,科目，成绩)
select t1.stuid, t1.name, t1.subject, t1.score from stuscore t1,
(select subject, MAX(score) as maxscore from stuscore group by subject) t2
where t1.subject = t2.subject and t1.score = t2.maxscore

// 6.列出各门课程成绩最好的两位学生(要求显示字段: 学号，姓名,科目，成绩)
select distinct t1.* from stuscore t1 where t1.stuid in
(select top 2 stuid from stuscore t2 where t1.subject = t2.subject order by score desc) order by t1.subject

// 7.列出各门课程的平均成绩（要求显示字段：课程，平均成绩）
select  subject, AVG(score) AS avgscore from stuscore group by subject
```