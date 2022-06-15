# SQL-
本笔记为阿里云天池龙珠计划SQL训练营的学习内容，链接为：https://tianchi.aliyun.com/specials/promotion/aicampsql
# Task02：SQL 基础查询与排序

select语句基础
1.从表中选取数据
select column_name from table_name;

2.从表中选取符合条件的数据
select column_name from table_name
where <条件表达式>;

3.选取全部列
select * from table_name;

4.为列设定别名
select column_name1 as name
       column_name2 as "列名"
from table_name;
as可以省略

5.删除列中重复数据
select distinct column_name from table_name;

6.SELECT子句中可以使用常数或者表达式。
使用比较运算符时一定要注意不等号和等号的位置。
字符串类型的数据原则上按照字典顺序进行排序，不能与数字的大小顺序混淆。
希望选取NULL记录时，需要在条件表达式中使用IS NULL运算符。希望选取不是NULL的记录时，需要在条件表达式中使用IS NOT NULL运算符。

7.not语句
select column_name from table_name
where not <条件表达式>;

8.选取null记录
select column_name from table_name
where column_name is null;

9.选取不为空的记录
select column_name from table_name
where column_name is not null;

10.and/or语句
当希望同时使用多个查询条件时，可以使用AND或者OR运算符。
AND 相当于“并且”，类似数学中的取交集；
OR 相当于“或者”，类似数学中的取并集。

聚合查询
1.聚合函数：SQL中用于汇总的函数，常用函数如下：
count：记录行数
sum：计算列中数值之和
avg：计算列中数值平均值
max：求列中数值最大值
min：求列中数值最小值
用法
select 聚合函数（column_name）from table_name;

2.计算全部数据的行数
select count(*) from table_name;

3.计算去除重复数据后的数据行数
select count(distinct column_name) from table_name;

4.常用法则
COUNT函数的结果根据参数的不同而不同。COUNT(*)会得到包含NULL的数据行数，而COUNT(<列名>)会得到NULL之外的数据行数。
聚合函数会将NULL排除在外。但COUNT(*)例外，并不会排除NULL。
MAX/MIN函数几乎适用于所有数据类型的列。SUM/AVG函数只适用于数值类型的列。
想要计算值的种类时，可以在COUNT函数的参数中使用DISTINCT。
在聚合函数的参数中使用DISTINCT，可以删除重复数据。

对表进行分组
1.group by语句用法
select column_name1,column_name2... from table_name
group by column_name1,column_name2....;

GROUP BY 子句就像切蛋糕那样将表进行了分组。在 GROUP BY 子句中指定的列称为聚合键或者分组列。
当聚合键中含null值时，group by会将其作为一组特殊数据进行处理

2.group by书写位置
select...from...where...group by...

3. 常见错误
在使用聚合函数及GROUP BY子句时，经常出现的错误有：
在聚合函数的SELECT子句中写了聚合健以外的列 使用COUNT等聚合函数时，SELECT子句中如果出现列名，只能是GROUP BY子句中指定的列名（也就是聚合键）。
在GROUP BY子句中使用列的别名 SELECT子句中可以通过AS来指定别名，但在GROUP BY中不能使用别名。因为在DBMS中 ,SELECT子句在GROUP BY子句后执行。
在WHERE中使用聚合函数 原因是聚合函数的使用前提是结果集已经确定，而WHERE还处于确定结果集的过程中，所以相互矛盾会引发错误。 
如果想指定条件，可以在SELECT，HAVING以及ORDER BY子句中使用聚合函数。

为聚合结果指定条件
1.用having得到特定分组
WHERE子句只能指定记录（行）的条件，而不能用来指定组的条件，HAVING子句用于对分组进行过滤，可以使用数字、聚合函数和GROUP BY中指定的列名（聚合键）。

对查询结果进行排序
1.order by语句用法
select column_name1,column_name2....from table_name
order by <排序基列1>,<排序基列2>,.....;

升序排列为asc(默认)，降序排列为desc，写在最后

2 ORDER BY中列名可使用别名
GROUP BY 子句中不能使用SELECT 子句中定义的别名，但是在 ORDER BY 子句中却可以使用别名。
为什么在GROUP BY中不可以而在ORDER BY中可以呢？
这是因为SQL在使用 HAVING 子句时 SELECT 语句的执行顺序为：
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY
其中SELECT的执行顺序在 GROUP BY 子句之后，ORDER BY 子句之前。
也就是说，当在ORDER BY中使用别名时，已经知道了SELECT设置的别名存在，但是在GROUP BY中使用别名时还不知道别名的存在，
所以在ORDER BY中可以使用别名，但是在GROUP BY中不能使用别名。

select语句的一般格式
select [all/distinct] <目标列表达式1>[别名1],<目标列表达式2>[别名2]....
from <table_name>,(select语句) [as] <别名>
[where<条件表达式>]
[group by <column_name> [having <条件表达式>]]
[order by <column_name> [asc/desc]]
1.目标列表达式的可选形式
*，<column_name>.*,count([all/distinct]*)
[column_name.]<属性列名表达式>...
