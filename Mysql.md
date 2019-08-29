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









## 持续更新中..

## 问题反馈
* 邮件:1297394526@qq.com
* QQ:1297394526
* Github: [@fangguizhen](https://github.com/fangguizhen)
