[TOC]

### 1.1.5主键

表中每一行都应该有可以唯一标识自己的一列（或一组列）。

唯一标识表中每行的这个列（或这组列）称为主键。

表中的任何列都可以作为主键，只要它满足以下条件：

* 任意两行都不具有相同的主键值；

* 每个行都必须具有一个主键值（主键列不允许NULL值）

主键通常定义在表的一列上，但也可以使用多个列作为主键。

### 3.2选择数据库

在执行任意数据库操作前，需要选择一个数据库。可使用USE关键字。

例如，为了使用crashcourse数据库，应该输入以下内容：

输入

```
USE crashcourse;
```

输出

```
Database changed
```

USE语句并不返回任何结果。这里显示出的Database changed消息是mysql命令行实用程序在数据库选择成功后显示的。

### 3.3了解数据库和表

如果你不知道可以使用的数据库名时怎么办？可用MYSQL到SHOW命令来显示这些信息。

输入

```
SHOW DATABASE;
```

输出

```
略
```

为了获得一个数据库内的表的列表，使用SHOW TABLES;

```
SHOW TABLES;
```

输出

```
略
```

SHOW TABLES;返回当前选择的数据库内可用表的列表。

SHOW也可以用来显示表列：

输入

```
SHOW COLUMNS FROM customers
```

输出

```
略
```

#### 自动增量

什么是自动增量？某些表列需要唯一值。例如，订单编号、雇员ID。

#### DESCRIBE语句

MYSQL支持用DESCRIBE作为SHOW COLUMNS FROM的一种快捷方式。

### 4.1SELECT语句

用途：从一个或者多个表中检索信息

为了使用SELECT检索表数据，必须至少给出两条信息——想选择什么，以及从什么地方选择。

### 4.2检索单个列

输入

```
SELECT name
FROM students;
```

#### 结束SQL语句

多条SQL语句必须以分号分隔

#### SQL语句和大小写

SQL语句不区分大小写，如果对SQL关键字使用大写，而对所有列和表名使用小写，这样子使代码更易于阅读和调试。

### 4.3检索多个列

在SELECT关键字后给出多个列名，列名之间必须以逗号分隔。（最后一个列名后不用加逗号）

输入

```
SELECT id,name,sex 
FROM students;
```

上面的语句将从students表中选择3列。

### 4.4检索所有列

可以通过在实际列名的位置使用星号（*）通配符来达到，如：

输入

```
SELECT *
FROM products;
```

分析

如果给定一个通配符（*），则返回表中所有列。

注意：一般，除非确实需要表中的每个列，否则最好别使用

*通配符。检索不需要的列通常会降低检索和应用程序的性能。

### 4.5检索不同的行

使用DISTINCT关键字，此关键字指示MYSQL中返回不同的值。

输入：

```
SELECT DISTINCT vend_id
FROM products;
```

只返回不同（唯一）的vend_id行

### 4.6限制结果

可使用LIMIT子句。

输入

```
SELECT prod_name
FROM products
LIMIT 5;
```

分析

LIMIT5指示MYSQL返回不多于5行。

为得出下一个5行，可指定要检索的开始行和行数。

输入

```
SELECT prod_name
FROM products
LIMIT 5.5;
```

分析

LIMIT5,5指示MYSQL返回从行5开始的5行。第一个数为开始位置，第二个数为要检索的行数。

* 总结

带一个值的LIMIT总是从第一行开始，给出的数为返回的行数。带两个值的LIMIT可以指定从行号为第一个值的位置开始。

* 注意

行0   检索出来的第一行为行0而不是行1。

### 4.7使用完全限定的表名

上面只是通过列名引用列，也可能会使用完全限定的名字来引用列（同时使用表名和列字）。

输入

```
SELECT products.prod_name
FROM products;
```

这里完全限定的列名。表名也可以是完全限定的。

### 5.1排序数据

为了明确地排序用SELECT语句检索出的数据，可使用ORDER BY子句。ORDER BY子句取一个或多个列的名字。

```
SELECT prod_name
FROM products
ORDER BY prod_name;
```

这条语句指示prod_name列以字母顺序排序。

### 5.2按多个列排序

经常需要按不止一个列进行数据排序。为了按多个列排序，只要指定列名，列名之间用逗号分开即可。

输入

```
SELECT prod_id, prod_price, prod_name 
FROM products;
ORDER BY prod_price, prod_name;
```

上面的代码检索3个列，并按其中两个列对结果进行排序--首先按价格，然后再按名称排序。

### 5.3 指定排序方向

#### 指定DESC关键字进行降序排序

输入

```
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price DESC;
```

如果打算用多个列排序，如

输入

```
SELECT prod_id, prod_price, prod_name
FROM products
ORDER BY prod_price DESC, prod_name;
```

上面的例子以降序排序产品（最贵的在前面），然后再对产品名排序。

分析

DESC关键字只应用到直接位于其前面的列名，在上面例子中，只对prod_price列指定DESC，对prod_name列不指定。如果想在多个列进行降序排序，必须对每个列指定DESC关键字

#### 指定ASC关键字进行升序排序

实际上，ASC没有多大用处，因为升序是默认的。

#### ORDER BY子句的位置

在给出ORDER BY子句时，应该保证它位于FROM子句后。如果使用LIMIT，它必须位于ORDER BY之后。使用子句的次序不对将产生错误信息。

### 6.1使用WHERE子句

数据库表一般包含大量的数据，很少才需要检索表中所有行。在SELECT语句中，数据根据WHERE子句中指定的搜索条件进行过滤。WHERE子句在表名（FROM子句）之后给出，如：

输入

```
SELECT prod_name,prod_price
FROM products
WHERE prod_price = 2.50
```

分析

这条语句从products表中检索两个列，但不是返回所有行，只返回prod_price值为2.5的行。

#### WHERE子句的位置

在同时使用ORDER BY和WHERE子句时，应该让ORDER BY位于WHERE之后。

#### WHERE子句操作符



------


参考：MYSQL必知必会
