# SQL备忘录



#### cheat sheet

![](https://s2.loli.net/2024/06/14/otczfC3An5vL8GN.jpg)



#### **SQL通配符**

| 通配符                           |            描述            |
| :------------------------------- | :------------------------: |
| %                                |     替代一个或多个字符     |
| _                                |       仅替代一个字符       |
| [charlist]                       |   字符列中的任何单一字符   |
| `[^charlist]` 或者 `[!charlist]` | 不在字符列中的任何单一字符 |

摘自：[W3School - 通配符](https://www.w3school.com.cn/sql/sql_wildcards.asp)



#### SQL 注释

SQL 注释分为<font color=FF0000>单行注释</font>和<font color=FF0000>块注释</font>

##### 嵌入行内的单行注释

```sql
-- 单行注释符号位"--"
```

> 💡 关于单行注释后面是否要加空格的问题：
>
> 比如 typora 的 代码块中，不加空格，将无法识别为注释，询问 chatgpt 的结果：
>
> <img src="https://s2.loli.net/2023/03/30/2hC1HT49o3RFPjJ.png" alt="image-20230330183050812" style="zoom:50%;" />

##### 块注释

```sql
/*
  块注释符号/*...*/
*/
```

摘自：[SQL单行注释，块注释](https://blog.csdn.net/qq_29656961/article/details/56842821)



#### ALTER TABLE 语句

ALTER TABLE 语句用于在已有的表中添加、修改或删除列。

##### 如需在表中添加列

```sql
ALTER TABLE table_name
ADD column_name datatype
```

##### 要删除表中的列

```sql
ALTER TABLE table_name 
DROP COLUMN column_name
```

> ⚠️ 某些数据库系统不允许这种在数据库表中删除列的方式 (DROP COLUMN column_name)。

##### 要改变表中列的数据类型

```sql
ALTER TABLE table_name
ALTER COLUMN column_name datatype
```

摘自：[W3School - SQL ALTER TABLE 语句](https://www.w3school.com.cn/sql/sql_alter.asp)

> 💡 补充

##### SQL 中 ALTER 和 UPDATE 的区别

ALTER 是DDL（Data Definition Language 数据定义语言）语句，是修改数据库中对象（表，数据库，视图...）的语句

UPDATE 是DML（Data Manipulation Language 数据操控语言）语句，是修改表中数据的语句

摘自：[sql 中 ALTER 和 UPDATE 的区别](https://www.cnblogs.com/zjfjava/p/6698591.html)



#### SQL identity函数

> 💡 属于 SQL Server

##### 含义

identity表示该字段的值会自动更新，不需要我们维护，通常情况下我们不可以直接给identity修饰的字符赋值，否则编译时会报错。

##### 语法

```sql
列名 数据类型 约束 identity(m,n)
```

<font color=LightSeaGreen>m 表示的是初始值，n 表示的是每次自动增加的值</font>；如果 m 和 n 的值都没有指定，默认 m = 1, n = 1。要么同时指定 m 和 n 的值，要么 m 和 n 都不指定，不能只写其中一个值，不然会出错

##### 示例

```sql
create table student2(
    sid int primary key identity(20,5),
    sname nchar(8) not null,
    ssex nchar(1)
)
-- 可以省略列名insert into student2 values ('王五','女');
insert into student2(sname,ssex) values ('张三','男');
insert into student2 values ('李四','女');
```

摘自：[SQL Server中identity(自增)的用法](https://blog.csdn.net/tswc_byy/article/details/81747159)

一般情况下，当数据表中，若一列被设置成了标识列之后，是无法向标识列中手动的去插入标识列的显示值。但是，可以通过设置SET IDENTITY_INSERT属性来实现对标识列中显示值的手动插入。

##### 写法

- `SET IDENTITY_INSERT 表名 ON` ：表示开启对<font color=LightSeaGreen>标识列</font>显示值插入模式，允许对标识列显示值进行手动插入数据。

- `SET IDENTITY_INSERT 表名 OFF`：表示关闭对标识列显示值的插入操作，标识列不允许手动插入显示值。

> ⚠️ <font color=LightSeaGreen>`IDENTITY_INSERT` 的开启 `ON` 和 关闭 `OFF` 是**成对出现**的，**所以，在执行完手动插入操作之后，记得一定要把 `IDENTITY_INSERT` 设置为 `OFF`，否则下次的自动插入数据会插入失败**。</font>

摘自：[sql IDENTITY_INSERT对标识列的作用和详解](https://blog.csdn.net/qq112212qq/article/details/84634774)



#### 标识列

标识列：是SQL Server中的标识列又称标识符列，习惯上又叫自增列。标识列的创建与修改，通常在企业管理器和用Transact-SQL语句都可实现。

##### 该种列具有以下三种特点

1、列的数据类型为不带小数的数值类型

2、在进行插入 ( Insert ) 操作时，该列的值是由系统按一定规律生成，不允许空值

3、列值不重复，具有标识表中每一行的作用，每个表只能有一个标识列。

摘自：[百度百科 - 标识列](https://baike.baidu.com/item/标识列/11024704?fr=aladdin)



#### DDL / DML / DCL 区别

##### DDL

Data Definition Language 数据定义语言，用于<font color=red>**操作对象和对象的属性，这种对象包括数据库本身，以及数据库对象，像：表、视图等等**</font>，DDL 对这些对象和属性的管理和定义具体表现在 <font color=LightSeaGreen>**create、drop 和 alter**</font>上。特别注意：DDL 操作的“对象”的概念，”对象“包括对象及对象的属性，而且对象最小也比记录大个层次。

**DML** 

Data Manipulation Language 数据操控语言，用于操作数据库对象中包含的数据，也就是说<font color=red>**操作的单位是记录**</font>。

DML 的主要语句（ 操作）：insert、delete、update

**DCL**

Data Control Language 数据控制语句 的操作是数据库对象的权限，这些操作的确定使数据更加的安全。DCL 的操作对象是用户，这里的用户指的是数据库用户。DCL 的主要语句（操作）有：

- Grant 语句：允许对象的创建者给某用户或某组或所有用户 ( PUBLIC ) 某些特定的权限。
- Revoke 语句：可以废除某用户或某组或所有用户访问权限

摘自：[DDL/DML/DCL区别概述](https://www.cnblogs.com/kawashibara/p/8961646.html)



#### SQL limit 和 offset

limit 是属于 mysql 的语法，<font color=LightSeaGreen>用来从某个值开始，取出之后的N条数据的语法</font>。<font color=LightSeaGreen>用于限制查询结果返回的数量，常用于分页查询</font>

##### limit 有两种方式

- `limit a, b` ：后缀两个参数的时候（参数必须是一个整数常量），其中a是指记录开始的偏移量 ( offsset )，b 是指从第 a+1 条开始，取 b 条记录( size )。

- `limit b` ：后缀一个参数的时候，是直接取值到第多少位，类似于：`limit 0, b` 。

摘自：[SQL极限函数limit()详解](https://blog.csdn.net/u011277123/article/details/54844834)

##### offset 用法

`limit y offset x` ：跳过 x 条数据，读取 y 条数据

摘自：[SQL查询语句中的 limit 与 offset 的区别](https://blog.csdn.net/cnwyt/article/details/81945663)



#### SQL NOW() 函数

NOW 函数返回当前的日期和时间。示例如下：

```sql
SELECT NOW() FROM table_name
```

摘自：[SQL NOW() 函数](https://www.w3school.com.cn/sql/sql_func_now.asp)



#### SQL 的多种连接

<font size=4>**内连接（等值连接）、左外连接、右外连接、全连接、自然连接、自身连接、交叉连接**</font>

##### 内连接 

`inner join ... on ...` /  `join ... on ...` ，展现出来的是共同的数据

```sql
select m.Province,S.Name from member m inner join ShippingArea s on m.Province=s.ShippingAreaID
```
相当于：<font color=FF0000>等值连接</font>

```sql
select m.Province,S.Name from member m, ShippingArea s where m.Province=s.ShippingAreaID
```

##### 左连接（左外连接）  

`left join ... on ...` ，将返回左表的所有行。如果左表的某行在右表中没有匹配行，则将为右表返回空值左连接：

```sql
select m.Province, S.Name from member m left join ShippingArea s on m.Province=s.ShippingAreaID
```
<font color=LightSeaGreen>**以左表为主表，右表没数据为 null**</font>

##### 右连接（右外连接）

`right join ... on ...` ，将返回右表的所有行。如果右表的某行在左表中没有匹配行，则将为左表返回空值：右表为主表，左表中没数据的为 null

##### 全外连接

`full join ... on ...` ，<font color=LightSeaGreen>**完整外部联接返回左表和右表中的所有行。当某行在另一个表中没有匹配行时，则另一个表的选择列表列包含空值。**</font>如果表之间有匹配行，则整个结果集行包含基表的数据值。

```sql
select m.Province,S.Name from member m full join ShippingArea s on m.Province=s.ShippingAreaID
```

##### 自然连接

`natural join`，自然连接 ( Natural join ) 是一种特殊的等值连接，<font color=FF0000>不用指定连接列，也不能使用ON语句</font>。它要求两个关系中进行比较的分量<font color=FF0000>必须是相同的属性组</font>，<font color=red>**并且在结果中把重复的属性列去掉**</font>。而等值连接并不去掉重复的属性列。

###### 举例

> 如 A 中 a, b, c 字段，B 中有 c, d 字段，则
>
> ```sql
> select * from A natural join B
> ```
>
> 相当于
>
> ```sql
> select A.a, A.b, A.c, B.d from A.c = B.c
> ```
>
>  摘自：[自然连接两个表是怎么连接的，举例详细说明一下谢谢 -- lxy1204231633的答案](https://zhidao.baidu.com/question/498844735021942444.html)

##### 交叉连接

`cross join`，不能用加上 `on`

>  <font color=FF0000>交叉连接不带WHERE子句</font>，它<font color=FF0000>返回被连接的两个表所有数据行的笛卡尔积</font>，返回结果集合中的数据行数等于第一个表中符合查询条件的数据行数乘以第二个表中符合查询条件的数据行数。
>
> 摘自： [交叉连接 - 百度百科]([https://baike.baidu.com/item/交叉连接/5922457](https://baike.baidu.com/item/交叉连接/5922457)) 

以下说法存在异议，与上面百度百科定义矛盾：

###### 不带where

```sql
select *from T_student cross join T_class  -- cross join 可以省略不写，等于如下：
select *from T_student, T_class
```

###### 带where

往往会先生成两个表行数乘积的数据表，然后才根据where条件从中选择；和等值连接一样。

```sql
select * from T_student s cross join T_class c where s.classId = c.classId
-- 注:cross join后加条件只能用where,不能用on
```

###### 自身连接

> 连接操作不仅可以在两个表之间进行，也可以是一个表与其自己进行连接，称为表的自身连接
>
> 摘自《数据库系统概论》第五版 P99 自身连接

示例：

```sql
/*
  摘自：《数据库系统概论》第五版 P100
*/
select FIRST.Cno, SECOND.Cpno
from Cource FIRST, Cource SECOND
where FIRST.Cpno = SECOND.Cno
```

以上节选自：[SQL的四种连接-左外连接、右外连接、内连接、全连接](https://www.cnblogs.com/webwangjie/p/11425632.html)   [交叉连接 - 百度百科]([https://baike.baidu.com/item/交叉连接/5922457](https://baike.baidu.com/item/交叉连接/5922457))  [SQL的连接分为三种：内连接、外连接、交叉连接](https://blog.csdn.net/weixin_39241397/article/details/79772379)



#### constraints（约束）

##### SQL 中，我们有如下约束

- **NOT NULL** - 指示某列不能存储 NULL 值。
- **UNIQUE** - 保证某列的每行必须有唯一的值。
- **PRIMARY KEY** - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- **FOREIGN KEY** - 保证一个表中的数据匹配另一个表中的值的参照完整性。
- **CHECK** - <font color=red>保证列中的值符合指定的条件</font>。
- **DEFAULT** - 规定没有给列赋值时的默认值

摘自：[SQL 约束（Constraints）](https://www.runoob.com/sql/sql-constraints.html)

**同时，<font color=FF0000>Constraints也是关键字</font>，用于标识<font color=FF0000>“表级完整性约束条件”</font>**，语法如下：

```sql
create table tbl_name(
    -- table define...
    -- ...
    constraint constraint_name constraint_type(column字段名[, more column]) [more supplement]
    [more constraint definite]
    )
```

##### 使用 constraint 关键字的目的

使用 `constraint constraint_name` 就是为 `constraint_type(column字段名[, more column])` 建立一个名字，这样我们就能删除这个约束或者修改这个约束。

参考：[SQL中的CONSTRAINT用法总结](https://blog.csdn.net/shaderdx/article/details/77184924) 和 [sql中的constraint关键字](https://segmentfault.com/q/1010000000628676)



#### distinct关键字

在表中，一个列可能会包含多个重复值，有时您也许希望仅仅列出不同（distinct）的值。

DISTINCT 关键词用于返回唯一不同的值。

##### SQL SELECT DISTINCT 语法

```sql
SELECT DISTINCT column_name,column_name
FROM table_name;
```

摘自：[RUNOOB - SQL SELECT DISTINCT 语句](https://www.runoob.com/sql/sql-distinct.html)



#### having关键字

在 SQL 中增加 HAVING 子句原因是，WHERE 关键字无法与聚合函数一起使用。

HAVING 子句可以让我们筛选分组 ( `group by` ) 后的各组数据。

##### 语法

```sql
SELECT column_name, aggregate_function(column_name)
FROM table_name
WHERE column_name operator value
GROUP BY column_name
HAVING aggregate_function(column_name) operator value;
```

摘自：[RUNOOB - SQL HAVING 子句](https://www.runoob.com/sql/sql-having.html)



#### union关键字

SQL UNION 操作符合并两个或多个 SELECT 语句的结果。

请注意，<font color=FF0000>UNION 内部的每个 SELECT 语句必须拥有相同数量的列。列也必须拥有相似的数据类型。同时，每个 SELECT 语句中的列的顺序必须相同</font>。

##### SQL UNION语法

```sql
SELECT column_name(s) FROM table1
UNION
SELECT column_name(s) FROM table2;
```

默认地，<font color=FF0000>UNION 操作符选取不同的值</font>。<font color=FF0000>如果允许重复的值，请使用 UNION ALL</font>。

##### SQL UNION ALL 语法

```sql
SELECT column_name(s) FROM table1
UNION ALL
SELECT column_name(s) FROM table2;
```

> ⚠️ UNION 结果集中的<font color=FF0000>列名总是等于 UNION 中**第一个 SELECT 语句中的列名**</font>。

[RUNOOB - SQL UNION 操作符](https://www.runoob.com/sql/sql-union.html)

> 💡其他类似的运算符还有：except（差集）、intersect（交集），但是mysql似乎只支持 uion



#### TRUNCATE 清空表操作

<font color=FF0000>TRUNCATE TABLE 语句比DELET删除表中的所有行更快</font>。从逻辑上讲，<font color=FF0000>TRUNCATE TABLE它类似于DELETE没有WHERE子句的语句</font>。

TRUNCATE TABLE语句从表中删除所有行，但<font color=FF0000>**表结构及其列，约束，索引等保持不变**</font>。要删除表及其数据，可以使用该DROP TABLE语句，但是需谨慎操作。

##### 基本语法

```sql
TRUNCATE TABLE table_name;
```

##### TRUNCATE TABLE 和 DELETE 的区别

尽管 DELETE 和 TRUNCATE TABLE 似乎具有相同的效果，但是<font color=FF0000>它们的工作方式不同</font>。这是这两个语句之间的一些主要区别：

- TRUNCATE TABLE 语句删除并重新创建表，并使任何自动增量值都重置为其初始值（通常为1）。

- DELETE 可让您根据可选 WHERE 子句过滤要删除的行，TRUNCATE TABLE 而不支持 WHERE 子句则仅删除所有行。

- <font color=FF0000>TRUNCATE TABLE 与 DELETE 相比，它更快并且使用的系统资源更少</font>，<font color=LightSeaGreen>因为 DELETE 扫描表以生成受影响的行数，然后逐行删除行，并为每个删除的行在数据库日志中记录一个条目，而 TRUNCATE TABLE 只删除所有行而不提供任何其他信息。</font>

摘自：[SQL 清空表(TRUNCATE TABLE 语句)](https://www.nhooo.com/sql/sql-truncate-table-statement.html)



#### SQL聚合函数

聚合函数是<font color=LightSeaGreen>对一组值执行计算并返回单一的值的函数</font>，它经常与SELECT语句的GROUP BY子句一同使用；包含：

##### AVG

返回指定组中的平均值，空值被忽略。

```sql
select prd_no, avg(qty) from sales group by prd_no
```

##### COUNT

返回指定组中项目的数量。

```sql
select count(prd_no) from sales 
```

##### MAX

返回指定数据的最大值。

```sql
select prd_no, max(qty) from sales group by prd_no 
```

##### MIN

返回指定数据的最小值。

```sql
select prd_no, min(qty) from sales group by prd_no
```

##### SUM

返回指定数据的和，只能用于数字列，空值被忽略。

```sql
select prd_no, sum(qty) from sales group by prd_no
```

##### COUNT_BIG

返回指定组中的项目数量，与 COUNT 函数不同的是 COUNT_BIG 返回 bigint 值，而 COUNT 返回的是 int 值。

```sql
select count_big(prd_no) from sales
```

##### GROUPING

产生一个附加的列，当用CUBE或ROLLUP运算符添加行时，输出值为1；当所添加的行不是由CUBE或ROLLUP产生时，输出值为0.

```sql
select prd_no, sum(qty),grouping(prd_no) from sales group by prd_no with rollup
```

##### BINARY_CHECKSUM

返回对表中的行或表达式列表计算的二进制校验值，用于检测表中行的更改。

```sql
select prd_no, binary_checksum(qty) from sales group by prd_no
```

##### CHECKSUM_AGG

返回指定数据的校验值，空值被忽略。

```sql
select prd_no, checksum_agg(binary_checksum(*)) from sales group by prd_no
```

##### CHECKSUM

返回在表的行上或在表达式列表上计算的校验值，用于生成哈希索引。

##### STDEV

返回给定表达式中所有值的统计标准偏差。

```sql
select stdev(prd_no) from sales
```

##### STDEVP

返回给定表达式中的所有值的填充统计标准偏差。

```sql
select stdevp(prd_no) from sales
```

##### VAR

返回给定表达式中所有值的统计方差。

```sql
select var(prd_no) from sales
```

##### VARP

返回给定表达式中所有值的填充的统计方差。

```sql
select varp(prd_no) from sales
```

摘自：[sql中的聚合函数](https://blog.csdn.net/qq_39209045/article/details/81093734)



#### 其他SQL函数

##### FIRST() / LAST()

###### FIRST()

函数返回指定的字段中<font color=LightSeaGreen>第一个记录的值</font>，可使用 ORDER BY 语句对记录进行排序

摘自：[W3School - SQL FIRST() 函数](https://www.w3school.com.cn/sql/sql_func_first.asp)

###### LAST()

函数返回指定的字段中<font color=LightSeaGreen>最后一个记录的值</font>，可使用 ORDER BY 语句对记录进行排序

摘自：[W3School - SQL LAST() 函数](https://www.w3school.com.cn/sql/sql_func_last.asp)



#### regexp



#### Schema

//TODO

https://blog.csdn.net/u010429286/article/details/79022484