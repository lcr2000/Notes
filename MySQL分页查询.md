##### MySQL分页查询

一、sql语句：select * from table limit startPageNum,everyPageNum

1)语句解析：

table:你要查询的表

startPageNum：从多少条开始  

everyPageNum:每页多少条数

二、以上的sql语句可以写成如下：

select * from table limit **(page\*everyPageNum)**,everyPageNum

2)语句解析：

table:你要查询的表

page：第几页

everyPageNum:每页多少条数

**(page\*everyPageNum)=startPageNum**

三、重点：关键字**limit**

总结：以上两条语句都可以 区别是一个是在程序里面算出起始条数 ，另一个是在sql语句里面算出条数 ，个人建议还是在程序里面算好了再带入数据库查询。