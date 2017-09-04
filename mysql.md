## MYSQL ##

### 进入mysql #

	mysql -u root -p;

### 创建数据库

	CREATE DATABASE BASENAME;

### 显示所有数据库 #

	SHOW DATABASES;

### use相关数据库 #

	use basename 

### 导入sql文件（在相关路径下）#

	source sqlname(文件名包括后缀)

### 显示所有数据表

	SHOW TABLE;

### 显示数据表的属性，属性类型，主键信息 ，是否为 NULL，默认值等其他信息。

	SHOW COLUMNS FROM tabelname;

### 显示数据表的详细索引信息，包括PRIMARY KEY（主键）。

	SHOW INDEX FROM tabelname;

### 该命令将输出Mysql数据库管理系统的性能及统计信息。

	mysql> SHOW TABLE STATUS  FROM RUNOOB;   # 显示数据库 RUNOOB 中所有表的信息
	mysql> SHOW TABLE STATUS from RUNOOB LIKE 'runoob%';     # 表名以runoob开头的表的信息
	mysql> SHOW TABLE STATUS from RUNOOB LIKE 'runoob%'\G;   # 加上 \G，查询结果按列打印

### 删除数据库

	drop database databasename;

### 创建数据表

	CREATE TABLE table_name (column_name column_type);

### 删除数据表

	DROP TABLE table_name ;

### 插入数据

	INSERT INTO table_name ( field1, field2,...fieldN )
                       VALUES
                       ( value1, value2,...valueN );

### 查询数据

	SELECT column_name,column_name
	FROM table_name
	[WHERE Clause]
	[OFFSET M ][LIMIT N]

### WHERE 子句

	SELECT field1, field2,...fieldN FROM table_name1, table_name2...
	[WHERE condition1 [AND [OR]] condition2.....

### UPDATE 查询

	UPDATE table_name SET field1=new-value1, field2=new-value2
	[WHERE Clause]	

### DELETE 语句

	DELETE FROM table_name [WHERE Clause]

### LIKE 子句

	SELECT field1, field2,...fieldN 
	FROM table_name
	WHERE field1 LIKE condition1 [AND [OR]] filed2 = '%somevalue%'

### 排序

	SELECT field1, field2,...fieldN FORM table_name1, table_name2...
	ORDER BY field1, [field2...] [ASC [DESC]]

### GROUP BY 语法

	SELECT column_name, function(column_name)
	FROM table_name
	WHERE column_name operator value
	GROUP BY column_name;

### join 链接的使用

-	INNER JOIN（内连接,或等值连接）：获取两个表中字段匹配关系的记录。

		SELECT a.runoob_id, a.runoob_author, b.runoob_count 
		FROM runoob_tbl a INNER JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
		相当于
		SELECT a.runoob_id, a.runoob_author, b.runoob_count 
		FROM runoob_tbl a, tcount_tbl b WHERE a.runoob_author = b.runoob_author;

-	LEFT JOIN（左连接）：获取左表所有记录，即使右表没有对应匹配的记录。
	
		SELECT a.runoob_id, a.runoob_author, b.runoob_count 
		FROM runoob_tbl a LEFT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;

-   RIGHT JOIN（右连接）： 与 LEFT JOIN 相反，用于获取右表所有记录，即使左表没有对应匹配的记录。  
   
		SELECT a.runoob_id, a.runoob_author, b.runoob_count 
		FROM runoob_tbl a RIGHT JOIN tcount_tbl b ON a.runoob_author = b.runoob_author;
	
#### MySQL NULL 值处理

-	IS NULL: 当列的值是 NULL,此运算符返回 true。
-	IS NOT NULL: 当列的值不为 NULL, 运算符返回 true。
-	<=>: 比较操作符（不同于=运算符），当比较的的两个值为 NULL 时返回 true。
-	查找数据表中 runoob_test_tbl 列是否为 NULL，必须使用 IS NULL 和 IS NOT NULL，

		SELECT * FROM runoob_test_tbl WHERE runoob_count IS NULL;
		SELECT * from runoob_test_tbl WHERE runoob_count IS NOT NULL;

### MySQL 事务

- 在 MySQL 中只有使用了 Innodb 数据库引擎的数据库或表才支持事务。
- 事务处理可以用来维护数据库的完整性，保证成批的 SQL 语句要么全部执行，要么全部不执行。
- 事务用来管理 insert,update,delete 语句
- 一般来说，事务是必须满足4个条件（ACID）： Atomicity（原子性）、Consistency（稳定性）、Isolation（隔离性）、Durability（可靠性）
	- 1、事务的原子性：一组事务，要么成功；要么撤回。
	- 2、稳定性 ：有非法数据（外键约束之类），事务撤回。
	- 3、隔离性：事务独立运行。一个事务处理后的结果，影响了其他事务，那么其他事务会撤回。事务的100%隔离，需要牺牲速度。
	- 4、可靠性：软、硬件崩溃后，InnoDB数据表驱动会利用日志文件重构修改。可靠性和高速度不可兼得， innodb_flush_log_at_trx_commit 选项 决定什么时候吧事务保存到日志里。

##### 事物控制语句：

- `BEGIN` 或 `START TRANSACTION`；显式地开启一个事务；
- `COMMIT`；也可以使用 `COMMIT WORK`，不过二者是等价的。`COMMIT` 会提交事务，并使已对数据库进行的所有修改称为永久性的；
- `ROLLBACK`；有可以使用 `ROLLBACK WORK`，不过二者是等价的。回滚会结束用户的事务，并撤销正在进行的所有未提交的修改；
- `SAVEPOINT identifier`；`SAVEPOINT` 允许在事务中创建一个保存点，一个事务中可以有多个 `SAVEPOINT`；
- `RELEASE SAVEPOINT identifier`；删除一个事务的保存点，当没有指定的保存点时，执行该语句会抛出一个异常；
- `ROLLBACK TO identifier`；把事务回滚到标记点；
- `SET TRANSACTION`；用来设置事务的隔离级别。`InnoDB` 存储引擎提供事务的隔离级别有`READ UNCOMMITTED`、`READ COMMITTED`、`REPEATABLE READ`和`SERIALIZABLE`。

#### MYSQL 事务处理主要有两种方法：

1. 用 BEGIN, ROLLBACK, COMMIT来实现
	1. BEGIN 开始一个事务
	2. ROLLBACK 事务回滚
	3. COMMIT 事务确认
2. 直接用 SET 来改变 MySQL 的自动提交模式:
	1. SET AUTOCOMMIT=0 禁止自动提交
	2. SET AUTOCOMMIT=1 开启自动提交

### MySQL ALTER命令

   当我们需要修改数据表名或者修改数据表字段时，就需要使用到`MySQL ALTER`命令。

   删除，添加或修改表字段:  
   *如果数据表中只剩余一个字段则无法使用`DROP`来删除字段。*

	mysql> ALTER TABLE testalter_tbl  DROP i;
	mysql> ALTER TABLE testalter_tbl ADD i INT;

   修改字段类型及名称:  
   _如果需要修改字段类型及名称, 你可以在ALTER命令中使用 MODIFY 或 CHANGE 子句 。_

	ALTER TABLE testalter_tbl MODIFY c CHAR(10);
	mysql> ALTER TABLE testalter_tbl CHANGE field_name new_field_name BIGINT;
   
   ALTER TABLE 对 Null 值和默认值的影响
   当你修改字段时，你可以指定是否包含只或者是否设置默认值。

	mysql> ALTER TABLE testalter_tbl 
	-> MODIFY j BIGINT NOT NULL DEFAULT 100;

   修改字段默认值

	mysql> ALTER TABLE testalter_tbl ALTER i SET DEFAULT 1000;
	mysql> ALTER TABLE testalter_tbl ALTER i DROP DEFAULT;

   修改表名

	mysql> ALTER TABLE testalter_tbl RENAME TO alter_tbl;

   alter其他用途：  

- 修改存储引擎：修改为myisam

		alter table tableName engine=myisam;

- 删除外键约束：keyName是外键别名

		alter table tableName drop foreign key keyName;

- 修改字段的相对位置：这里name1为想要修改的字段，type1为该字段原来类型，first和after二选一，这应该显而易见，first放在第一位，after放在name2字段后面

		alter table tableName modify name1 type1 first|after name2; 