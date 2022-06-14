---
title: 📒【MySQL】MySQL 基础操作
date: 2021-06-20
categories: 数据库
comments: true
tag:  MySQL
toc: true
---

数据库（database） 保存有组织的数据的容器
表（table） 某种特定类型数据的结构化清单。

行（row）： 表中的一个记录。
> 记录和行可以替换，从技术上说行才是正确的术语。

主键（primary key）：列（或一组列），其值能够唯一区分表 中每个行。没有主键，更新或删除表中特定行很困难，因为没有安全的方法保证只涉及相关的行。

SQL是结构化查询语言（Structured Query Language）的缩写。

## 动手实践
启动MySQL服务
`sudo /usr/local/MySQL/support-files/mysql.server start`

停止MySQL服务
`sudo /usr/local/mysql/support-files/mysql.server stop`

重启MySQL服务
`sudo /usr/local/mysql/support-files/mysql.server restart`

进入Mysql之后
- 命令用;或`\g`结束
- help或`\h`获得帮助
- quit或exit退出命令实用程序 

显式数据库名
`show databases;`

使用数据库
`use name-of-database;`

显示表
`show tables;`

显示表的列
`show columns from column-name;`
或者使用快捷方式`describe column-name`

其他show
```sql
show status # 显示服务器状态信息
show create database # 显示创建特定数据库的mysql语句
show create table # 显示创建特定表的语句
```
### 检索数据 - 查
> SQL语句不区分大小写，因此 SELECT与select是相同的。同样，写成Select也没有关系。 许多SQL开发人员喜欢对所有SQL关键字使用大写，而对所有 列和表名使用小写，这样做使代码更易于阅读和调试。

- 检索一列`select column-name from table`
- 检索多个列在column-name中间添加`,`
- 检索所有列`select * from table`
- 检索不同的行`select distinct column-name from table`，只会显示这一列中值唯一的行
- 限制返回行的数量`select column-name from table limit 5`
	- `select column-name from table limit 5,5`返回从第一个值的行开始的第二个值行数，为了避免弄错，可以使用语法`select column-name from table limit 5 offset 5`
- 使用完全限定的表明`select table-name.column-name from database-name.table`

- 数据排序`select column-name from table order by column-name`，会按照字母或者数字大小排序
- 多个列排序`select column-name1,column-name2 from table order by column-name1,column-name2`只有在第二个参数具有相同的值时再进行按第二个值的排序
- 指定排序方向`select column-name from table order by column-name desc`降序，默认是升序，所以不需要`asc`参数

过滤数据
- 搜索条件的过滤`select column-name1,column-name2 from table where column=char-or-num`
	- WHERE和order by同时使用应该让order by 位于 where之后

- ![[Pasted image 20210728202238.png]]
	- `select prod_name,prod_price from products where prod_name="fuses";`
	- `select prod_name,prod_price from products where prod_price<=10;`
	- 不匹配检查 `select column-name from table where column-name <> value-not-in-table` 或者`!=`符号
	- 范围检查`where column-name between num and num`
	- 空值检查`where column-name is NULL` 

组合where
- 使用and给where添加附加条件`select * from table where col-name = num and col-name <= num`
- 使用or匹配其中一条原则
- 组合使用and和or时可以使用圆括号增加优先级
- 使用in (value, value)，括号中的每个条件都会进行匹配，它与or具有相同的功能
- NOT操作符，否定之后的所有条件

通配符过滤
- like操作符，`%`表示任何字符出现任意次数，`_`匹配单个字符，`select col-name from table where col-name like '%cha'`

正则表达式
- 和like的用法相似，但是告诉mysql这是正则表达式regexp,`select col-name from table where col-name like '%cha'`
- 使用or匹配，`select col-name from table where col-name like '1000｜2000'`
- 匹配任意单个字符`[123]`，`[^123]`匹配除这些字符以外的任何东西，匹配范围`[1-5]`，匹配特殊字符用转译`\\.`
- ![[Pasted image 20210729101611.png]]
- 匹配多个实例![[Pasted image 20210729101658.png]]
- 定位符![[Pasted image 20210729102225.png]]

创建计算字段
- 拼接concat()函数将值结合构成一个值，`select concat(vend_name, '(', vend_country,')') from vendors order by vend_name;`
- rtrim()函数移除右侧多余的空格，ltrim()去掉左侧的空格，trim()移除两侧的空格
- 使用别名，`select concat(vend_name, '(', vend_country,')') as vend_title from vendors order by vend_name;`
- 执行算术计算`select prod_id,quantity, item_price, quantity*item_price as expanded_price from orderitems where order_num = 20005;`

数据处理函数
- 前面提到的trim()，now()![[Pasted image 20210729103628.png]]
- 日期和时间处理函数![[Pasted image 20210729103811.png]]
- 数值处理函数![[Pasted image 20210729104248.png]]
- 汇总数据![[Pasted image 20210729104353.png]]

数据分组
- 创建分组`select vend_id, count(*) as num_prods from products group by vend_id;`
- 过滤分组，having支持所有where操作，句法一样，只是关键字有差别。where在分组前进行过滤，having在分组后进行过滤，`select cust_id, count(*) as orders from orders group by cust_id having count(*) >=2;`
- 子句顺序![[Pasted image 20210729110558.png]]

子查询
子查询总是从内向外处理，`select cust_id from orders  where order_num in (select order_num from orderitems where prod_id = 'TNT2');`
外键为某个表中的一列，它包含另一个表 的主键值，定义了两个表之间的关系。

笛卡儿积（cartesian product）:由没有联结条件的表关系返回 的结果为笛卡儿积。检索出的行的数目将是第一个表中的行数乘以第二个表中的行数。

sql对连接表的个数没有限制，连接表的数量越多，性能下降越多。
可以给表创建别名`select cust_name, cust_contact from customers as c,orders as o,orderitems as oi where c.cust_id = o.cust_id and oi.order_num = o.order_num and prod_id = 'TNT2';`

组合查询
将单个select查询的结果组合成一个结果，两条语句中间加上union即可。union会默认去除重复的行，使用union all匹配所有的行

## 增
向表中添加数据，
```sql
insert into customers
values(
	null,
	xxx,
	xxx
) #这种方式不安全，可能会出现问题，采用列出列的名字更安全
insert into customers(cust_name,
	cust_address,
	xx
)values(
	null,
	xxx,
	xxx
)#,(
	null,
	xxx,
	xxx
)	#可以一次添加多个值
insert into customers
values(
	null,
	xxx,
	xxx
)
select xxx from xxx; # 从选出数据插入
```

更新
- `update table-name set col-name = xx where col-name = xx`

## 删
- 删除表的内容`delete from table-name where col-name = xx`
- 删除所有行，`truncate table`

## 表操作
### 增
- 创建表
	- create table关键字后
	- 表的名字和定义
	- 一图胜千言![[Pasted image 20210729151727.png]]

更新表
一般在表定义完成之后就不再被更新。
```sql
alter table table-name
add col-name data-type;
drop COLUMN col-name;
```
### 删
删除表而不是内容`drop table-name`
重命名`rename TABLE table-name to a-table-name`

## 视图
视图包含的是某个查询的结果，不包含数据本身。
- 视图用CREATE VIEW语句来创建。 
- 使用SHOW CREATE VIEW viewname；来查看创建视图的语句。 
- 用DROP删除视图，其语法为DROP VIEW viewname;。 
- 更新视图时，可以先用DROP再用CREATE，也可以直接用CREATE OR REPLACE VIEW。如果要更新的视图不存在，则第2条更新语句会创建一个视图；如果要更新的视图存在， 则第 2条更新语句会替换原有视图。

## 触发器
创建触发器的条件：
- 唯一的触发器名； 
- 触发器关联的表； 
- 触发器应该响应的活动（DELETE、INSERT或UPDATE）； 
- 触发器何时执行（处理之前或之后）。
`create trigger newproduct after insert on products for each row select 'product added'`

删除`drop trigger trigger-name`

## 事务
事务 ：指一组sql语句
回退：撤销指定sql语句的过程
提交：将为存储的sql语句结果写入数据库表
保留点：事务处理中设置的临时占位符，可以对它发布回退

- rollback 只能在一个事务处理内使用![[Pasted image 20210729153921.png]]
- commit 提交
- savepoint point-name，回退使用rollback to point-name