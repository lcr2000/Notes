### Mysql

#### 1、Mysql事物4大特点

* 1、原子性：要么全部成功，要么全部失败。
* 2、一致性：事物内有一个操作失败时，所有更改过的数据都必须回滚到修改前的状态。
* 3、隔离性：事物查看数据时数据所处的状态，要么是修改之前的状态，要么是修改后的状态，事物不会查看中间状态的数据。
* 4、持久性

#### 2、使用DATE列方面的问题

DATE值的格式是'YYYY-MM-DD'。按照标准的SQL，不允许其他格式。在UPDATE表达式以及SELECT语句的WHERE子句中应使用该格式。例如：

```
SELECT * FROM tbl_name WHERE date >= '2003-05-05';
```

为了方便，如果日期是在数值环境下使用的，MySQL会自动将日期转换为数值（反之亦然）。它还具有相当的智能，在更新时或在与TIMESTAMP、DATE或DATETIME列比较日期的WHERE子句中，允许“宽松的”字符串形式（“宽松形式”表示，任何标点字符均能用作各部分之间的分隔符。例如，'2004-08-15'和'2004#08#15'是等同的）。MySQL还能转换不含任何分隔符的字符串（如'20040815'），前体是它必须是有意义的日期。

#### 3、mysql开启/关闭 update delete 安全模式

在使用mysql执行update的时候，如果不是用主键当where语句，会报如下错误，使用主键用于where语句中正常。

异常内容：Error Code: 1175. You are using safe update mode and you tried to update a table without a WHERE that uses a KEY column To disable safe mode, toggle the option in Preferences -> SQL Queries and reconnect.

 

因为MySql运行在safe-updates模式下，该模式会导致非主键条件下无法执行update或者delete命令，执行命令

```
SET SQL_SAFE_UPDATES = 0
```

安全起见，执行完操作后，建议在恢复成默认状态1

#### 4、Truncate用法

导读：删除表中的数据的方法有delete,truncate, 其中TRUNCATE TABLE用于删除表中的所有行，而不记录单个行删除操作。TRUNCATE TABLE 与没有 WHERE 子句的 DELETE 语句类似；但是，TRUNCATE TABLE 速度更快，使用的系统资源和事务日志资源更少。下面介绍SQL中Truncate的用法

**当你不再需要该表时， 用 drop；当你仍要保留该表，但要删除所有记录时， 用 truncate；当你要删除部分记录时（always with a WHERE clause), 用 delete.**

Truncate是一个能够快速清空资料表内所有资料的SQL语法。并且能针对具有自动递增值的字段，做计数重置归零重新计算的作用。

**一、Truncate语法**


[ { database_name.[ schema_name ]. | schema_name . } ]
    table_name
[ ; ]


**参数**


database_name
数据库的名称。


schema_name
表所属架构的名称。

table_name
要截断的表的名称，或要删除其全部行的表的名称。

**二、Truncate使用注意事项**

1、TRUNCATE TABLE 在功能上与不带 WHERE 子句的 DELETE 语句相同：二者均删除表中的全部行。但 TRUNCATE TABLE 比 DELETE 速度快，且使用的系统和事务日志资源少。

2、DELETE 语句每次删除一行，并在事务日志中为所删除的每行记录一项。TRUNCATE TABLE 通过释放存储表数据所用的数据页来删除数据，并且只在事务日志中记录页的释放。

3、TRUNCATE TABLE 删除表中的所有行，但表结构及其列、约束、索引等保持不变。新行标识所用的计数值重置为该列的种子。如果想保留标识计数值，请改用 DELETE。如果要删除表定义及其数据，请使用 DROP TABLE 语句。

4、对于由 FOREIGN KEY 约束引用的表，不能使用 TRUNCATE TABLE，而应使用不带 WHERE 子句的 DELETE 语句。由于 TRUNCATE TABLE 不记录在日志中，所以它不能激活触发器。

5、TRUNCATE TABLE 不能用于参与了索引视图的表。

6、对用TRUNCATE TABLE删除数据的表上增加数据时，要使用UPDATE STATISTICS来维护索引信息。

7、如果有ROLLBACK语句，DELETE操作将被撤销，但TRUNCATE不会撤销。

**三、不能对以下表使用 TRUNCATE TABLE**


1、由 FOREIGN KEY 约束引用的表。（您可以截断具有引用自身的外键的表。）


2、参与索引视图的表。


3、通过使用事务复制或合并复制发布的表。


4、对于具有以上一个或多个特征的表，请使用 DELETE 语句。

5、TRUNCATE TABLE 不能激活触发器，因为该操作不记录各个行删除。

**四、TRUNCATE、Drop、Delete区别**


1.drop和delete只是删除表的数据(定义),drop语句将删除表的结构、被依赖的约束(constrain)、触发器 (trigger)、索引(index);依赖于该表的存储过程/函数将保留,但是变为invalid状态。

3.delete语句不影响表所占用的extent、高水线(high watermark)保持原位置不动。drop语句将表所占用的空间全部释放。truncate语句缺省情况下将空间释放到minextents的 extent,除非使用reuse storage。truncate会将高水线复位(回到最初)。

4.效率方面:drop > truncate > delete

5.安全性:小心使用drop与truncate,尤其是在 没有备份的时候,想删除部分数据可使用delete需要带上where子句,回滚段要足够大,想删除表可以用drop,想保留表只是想删除表的所有数据、 如果跟事物无关可以使用truncate,如果和事物有关、又或者想触发 trigger,还是用delete,如果是整理表内部的碎片，可以用truncate跟上reuse stroage，再重新导入、插入数据。

6.delete是DML语句,不会自动提交。drop/truncate都是DDL语句,执行后会自动提交。

7、drop一般用于删除整体性数据 如表，模式，索引，视图，完整性限制等；delete用于删除局部性数据 如表中的某一元组

8、DROP把表结构都删了；DELETE只是把数据清掉

9、当你不再需要该表时， 用 drop；当你仍要保留该表，但要删除所有记录时， 用 truncate；当你要删除部分记录时（always with a WHERE clause), 用 delete.

**truncate:会清空表中所有的数据，速度快，不可回滚；实质是删除整张表包括数据再重新创建表；**

**delete:逐行删除数据，每步删除都是有日志记录的，可以回滚数据；实质是逐行删除表中的数据；**

#### 5、使用where 1 = 1

* 当要构造动态sql语句时为了防止sql语句结构不当，所以加上where 1=1
  where 1=1是永真条件
  所以就是select * from a,就会返回表a中所有数据

* 还有一种永假条件where 1=0或者where 1<>1
  有时候可能并不想获得数据，只想知道该表的结构就会返回所有列，但是没有记录（行）

争议：这看似非常优美的解决了问题，殊不知这样很可能会造成非常大的性能损失，因为使用添加了“1=1”的过滤条件以后数据库系统就无法使用索引等查询优化策略，数据库系统将会被迫对每行数据进行扫描（也就是全表扫描）以比较此行是否满足过滤条件，当表中数据量比较大的时候查询速度会非常慢。因此如果数据检索对性能有比较高的要求就不要使用这种“简便”的方式。（这段有待验证）































转自：https://www.cnblogs.com/zhoufangcheng04050227/p/7991759.html

原文：http://www.studyofnet.com/news/555.html

## 持续更新中..

## 问题反馈
* 邮件:1297394526@qq.com
* QQ:1297394526
* Github: [@fangguizhen](https://github.com/fangguizhen)
