# Hive操作

## 准备

准备[Hive的安装配置](https://zhuanlan.zhihu.com/p/500689765)

准备文件，可以是直接创建的也可以是从宿主机上传

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422093805.png)

确定启动hadoop和hive

参考[Hive语法文档](https://cwiki.apache.org/confluence/display/Hive/LanguageManual)



## DDL操作

### 显示已有的数据库

`show databases [like 'REGULAR_EXPRESSION']`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423091123.png)


### 创建数据库

`create database [if not exists] DATABASE_NAME;`

`[if not exists]`表示如果没有此数据库则创建

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422100710.png)



### 修改数据库属性

修改数据库的属性，`dbproperties`是数据库的属性信息，可以通过修改键值对的方式修改

`alter database DATABASE_NAME set dbproperties(key1=value1, key2=value2……);`



### 创建表
`use DATABASE_NAME;`
`create table TABLE_NAME(COL_NAME_1 TYPE, COL_NAME_2 TYPE …… );`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422102134.png)

使用自定义分隔符创建内部表

`create table TABLE_NAME(COL_NAME TYPE) row format delimited fields terminated by "\t";`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422104422.png)

创建表也可以通过查询结果创建（表的结构和数据）

`create table if not exists CREATE_TABLE_NAME as select COL1, COL2 from TABLE_NAME;`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423092239.png)

通过已有的表创建（表的结构）

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423092527.png)




### 指定表的数据

#### 通过hdfs

找到hadoop的存储数据库的文件夹`/user/hive/warehouse`（与自己的配置有关）

`hadoop fs -ls /user/hive/warehouse`或`hdfs dfs -ls /user/hive/warehouse`

数据文件是可以直接通过向此文件夹添加来实现填充数据的

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422101622.png)

#### hive中导入
**从虚拟机导入**

`load data local inpath "FILE_PATH_NAME" into table DATABASE_NAME.DATABASE_TABLE_NAME;`

命令向指定的表中添加数据

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422102920.png)

**从hdfs导入**

先将需要导入的数据文件上传到HDFS上
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422103429.png)

使用如下命令

`load data inpath "FILE_PATH_NAME" into table DATABASE_NAME.DATABASE_TABLE_NAME;`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422104107.png)


### 插入数据

`insert into TABLE_NAME(COL) values(data);`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422105056.png)

**将hdfs上指定的文件作为hive创建的外部表的数据**

`create external table DATABASE_NAME.TAB_NAME(COL TYPE) row format delimited fields terminated by '\t' location 'FILE_PATH';`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422105637.png)

**修改外部表的数据目录**

`alter table TABLE_NAME set location "HDFS_DIR_PATH";`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422110503.png)

**修改表属性**

将外部表属性修改为内部表

`alter table TABLE_NAME set tblproperties ("EXTERNAL"="FALSE");`

自带的属性键值对为固定写法（区分大小写）

将内部表转外部表

`alter table TABLE_NAME set tblproperties ("EXTERNAL"="TRUE");`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220422111232.png)


### 删除表

删除外部表不会删除hdfs的数据文件

`drop table TABLE_NAME;`


### 删除数据库

`drop database [if not exists] DATABASE_NAEM [cascade];`

参数cascade表示即便数据库不为空，也强制删除，如果不加只能删除空数据库

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423093654.png)


### 分区表

#### 数据
 
准备如下数据

分别存放在`/home/dept.txt`和`/home/emp.txt`中

```txt
10	ACCOUNTING	1700
20	RESEARCH	1800
30	SALES	1900
40	OPERATIONS	1700
```

```txt
7369	SMITH	CLERK	7902	1980-12-17	800.00		20
7499	ALLEN	SALESMAN	7698	1981-2-20	1600.00	300.00	30
7521	WARD	SALESMAN	7698	1981-2-22	1250.00	500.00	30
7566	JONES	MANAGER	7839	1981-4-2	2975.00		20
7654	MARTIN	SALESMAN	7698	1981-9-28	1250.00	1400.00	30
7698	BLAKE	MANAGER	7839	1981-5-1	2850.00		30
7782	CLARK	MANAGER	7839	1981-6-9	2450.00		10
7788	SCOTT	ANALYST	7566	1987-4-19	3000.00		20
7839	KING	PRESIDENT		1981-11-17	5000.00		10
7844	TURNER	SALESMAN	7698	1981-9-8	1500.00	0.00	30
7876	ADAMS	CLERK	7788	1987-5-23	1100.00		20
7900	JAMES	CLERK	7698	1981-12-3	950.00		30
7902	FORD	ANALYST	7566	1981-12-3	3000.00		20
7934	MILLER	CLERK	7782	1982-1-23	1300.00		10
```

#### 一级分区表

```sql
create table TABLE_NAME(COL1 TYPE, COL2 TYPE……) 
partitioned by (month TYPE) 
row format delimited dields terminated by '\t'
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423093950.png)

加载数据

`load data local inpath 'DATA_FILE_PATH' into table DATABASE_NAME.TABLE_NAME partition(KEY=VALUE);`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423094600.png)

#### 查询分区表中的数据

**单分区查询**

```sql
select * from TABLE_NAME where KEY=VALUE;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423094825.png)

**多分区联合查询**

实现VALUE1和VALUE2的联合查询

```sql
select * from TABLE_NAME where KEY=VALUE1;
union
select * from TABLE_NAME where KEY=VALUE2;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423094954.png)

**创建分区**

`alter table TBAME_NAME add partition(KEY=VALUE_1) [partition(KEY=VALUE_2)……];`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423095829.png)

**删除分区**

删除多个分区时，分区之间必须用英文逗号隔开

`alter table TABLE_NAME drop partition(KEY=VALUE_1) [,partition(KEY=VALUE_2)……];`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423100223.png)

`show partitions TABLE_NAME;`显示已有的分区

`desc formatted TABLE_NAME;`查看分区表结构

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423100409.png)

#### 二级分区表

```sql
create table TABLE_NAME(  
COL_1 TYPE, COL_2 TYPE, COL_3 TYPE  
)  
partitioned by (KEY VALUE_1, KEY VALUE_2)  
row format delimited fields terminated by '\t';
```

在以及分区month上创建二级分区day

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423100738.png)

**正常加载数据**

```sql
load data local inpath 'DATA_PATH' into table DATABASE_NAME.TABLE_NAME partition(KEY VALUE_1, KEY VALUE_2);
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423101255.png)

**把数据直接上传到分区目录上，让分区表和数据产生关联的三种方式**


1、上传数据后修复

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423101832.png)

此时只是上传数据，并不能查到上传的数据

修复命令，此时再次查询就可以查询到数据
```sql
msck repair table TABLE_NAME;
```

2、上传数据后分区

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423103227.png)

3、创建文件夹后加载数据到分区

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423103446.png)




### 表的其他操作

#### 修改表的结构

**重命名表**

```sql
alter table TBALE_NAME rename to NEW_TABLE_NAME;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423104228.png)

**添加列**

```sql
alter table TABLE_NAME add columns(
COL_1 TYPE [comment '描述'], 
COL_2 TYPE [comment '描述']
……);
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423104944.png)

**替换/更新列**

注意，会将表中的所有列换为更新的列，不支持只更新一个，如果需要只更新一个，那么需要将其他的都写进更新后的columns中

```sql
alter table TABLE_NAME replace columns(
COL_1 TYPE [comment '描述'], 
COL_2 TYPE [comment '描述']
……);
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423105617.png)

**清空表数据**

只能清空内部表的数据，如果需要清空外部表数据需要使用`shell`命令删除hdfs上的目录

```sql
truncate table TABLE_NAME;
```


### 分桶与抽样

#### 数据

在`/home/stu.txt`

```txt
1001	ss1
1002	ss2
1003	ss3
1004	ss4
1005	ss5
1006	ss6
1007	ss7
1008	ss8
1009	ss9
1010	ss10
1011	ss11
1012	ss12
1013	ss13
1014	ss14
1015	ss15
1016	ss16
```

#### 创建分桶表

**先创建普通的表**

```sql

create table stu(
id int,
name string)
row format delimited fields terminated by '\t';
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423110405.png)

**导入数据、创建分桶表**

N 为个数

```sql
create table TABLE_NAME(COL_1 TYPE, COL_2 TYPE)  
clustered by(id) into N buckets  
row format delimited fields terminated by '\t';
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423110641.png)

**向分桶表导入数据（通过子查询）**

```sql
insert into table TABLE_NAME_BUCK select * from TABLE_NAME;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423110946.png)

此时可以在主节点的50070端口查看到分桶成功

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423111152.png)

**抽样**

对于非常大的数据集，有时用户需要使用的是一个具有代表性的查询结果而不是全部结果。Hive可以通过对表进行抽样来满足这个需求。

```sql
select * from TABLE_NAME tablesample(范围);
```

`TABLESAMPLE(BUCKET x OUT OF y)`

y必须是table总bucket数的倍数或者因子。hive根据y的大小，决定抽样的比例。例如，table总共分了4份，当y=2时，抽取(4/2=)2个bucket的数据，当y=8时，抽取(4/8=)1/2个bucket的数据。

x表示从哪个bucket开始抽取，如果需要取多个分区，以后的分区号为当前分区号加上y。例如，table总bucket数为4，tablesample(bucket 1 out of 2)，表示总共抽取（4/2=）2个bucket的数据，抽取第1(x)个和第3(x+y)个bucket的数据。  

注意：x的值必须小于等于y的值，否则收到错误。

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423111624.png)



## DML操作

### 基本查询语句

**全表查询**

```sql
select * from TABLE_NAME;
```

**特定列查询**

```sql
select COL_1, COL_2…… from TABLE_NAME;
```


**查询给列设置别名**

```sql
select COL_1 as NEW_NAME, COL_2 NEW_NAME from TABLE_NAME;
```

**规定查询的行数**

从第 N 行开始，向下查找 M 行
```sql
select * from TABLE_NAME limit N offset M;
select * from TABLE_NAME limit N, M;
```






### where 语句

**比较运算符**

`A <=> B`：当 A 或 B 是NULL时结果为 NULL，其他只要相等都为True
`A <> B`：当 A 或 B 是NULL时结果为 NULL，与 `A != B` 相同

**like和rlike**

`_` 代表一个任意字符，`%` 代表任意个字符

```sql
select COL from COL like '%A%';  # 输出所有包含A的
select COL from COL rlike 'A';   # 输出所有包含A的
```


### 分组

`select COL_1, COL_2…… from TABLE_NAME[ where 筛选] group by COL_N[ having 筛选];`

* having与where不同点：
	* （1）where针对表中的列发挥作用，查询数据；having针对查询结果中的列发挥作用，筛选数据。
	* （2）where后面不能写分组函数，而having后面可以使用分组函数。
	* （3）having只用于groupby分组统计语句，不能单独使用


### join语句

hive支持join但是仅支持等值连接

```sql
# 支持
select name_1.COL_1, name_2.COL_2
from TABLE_NAME_1 name_1 join TABLE_NAME name_2 
on name_1.COL_N = name_2.COL_M;

# 不支持
select name_1.COL_1, name_2.COL_2
from TABLE_NAME_1 name_1 join TABLE_NAME name_2 
on name_1.COL_N 非等 name_2.COL_M;

非等：<、>、>=、<=、!>、!<、<>
```


### 排序

#### 全局排序

```sql
select * from TABLE_NAME [where 筛选] order by COL_N [, COL_M, COL_Q ……] [asc/desc];
# 默认为 asc 升序，desc 降序
```

#### mapreduce内部排序

`sort by`: 每个Reducer内部进行排序，对全局结果集来说不是排序
```sql
set mapreduce.job.reduces[ = N];  # 设置reduce个数不写[]是查看个数

# 使用方法同全局排序 
```

#### 分区排序

`Distribute By`：类似MR中partition，进行分区，结合`sort by`使用，必须`sort by`前面

#### cluster by

如果在使用分区排序时，`distribute by` 的字段和 `sort by` 字段相同，可以使用 `cluster by` ，同时具备两个排序的功能，但是不支持自定义升降序，只能是升序


### 常用查询函数

#### 空字段赋值

`nvl(COL, VALUE/COL)`

如果COL_1值为NULL那么返回VALUE或COL_2替代，反之返回COL_1

```sql
select nvl(COL_1, N/COL_2) from TABLE_NAME
```

#### case when

按需查询数据

```sql
# 查询表中字段为 COL_1 的如果值为 VALUE 那么返回 VALUE_1 否则返回 VALUE_2
select case COL_1 when VALUE then VALUE_1 else VALUE_2 end
from TABLE_NAME
```

#### 行转列

* ` CONCAT(stringA/col,stringB/col...)`：返回输入字符串连接后的结果，支持任意个输入字符串;
* `CONCAT_WS(separator,str1,str2,...)`：它是一个特殊形式的CONCAT()。第一个参数表示剩余参数间的分隔符。分隔符可以是与剩余参数一样的字符串。如果分隔符是NULL，返回值也将为NULL。这个函数会跳过分隔符参数后的任何NULL和空字符串。分隔符将被加到被连接的字符串之间;
* `COLLECT_SET(col)`：函数只接受基本数据类型，它的主要作用是将某字段的值进行去重汇总，产生array类型字段

#### 列转行

* `EXPLODE(col)`：将hive一列中复杂的array或者map结构拆分成多行。
* `LATERAL VIEW`：用于和split，e'xplode等UDTF一起使用，在将一列数据拆分成多行数据的基础上，可以对拆分的数据进行聚合


### 导入导出

#### 导入

**装载数据**

```sql
load data [local] inpath 'PAHT_FILE' overwrite 
into table TABLE_NAME [ partition ( partcol1 = VALUE_1,...)];
```

overwrite表示会覆盖原有的数据，否则表示在末尾添加数据

可以查看DDL中的指定表的数据查看更多

除此之外也可以用基本的`insert into`向已有的表中插入数据

#### 导出

在hive控制台将查询结构导出

```sql
insert overwrite [local] directory 'PATH' 
[ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t']
SELECT_SQL;
```

`ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'`表示格式化导出间隔为`\t`
`local`表示导出到本地，如果不写就是导出到hdfs上

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220423112019.png)

在hive控制台使用hadoop命令导出使用Hadoop的`get`命令
也可以使用`export table DATABASE.TABLE to 'PATH_FILE_NAME';`导出表完整的数据

在宿主机的命令控制台使用

```sh
hive -e 'SELECT_SQL' > PATH_FILE_NAME;
```

将指定查询数据导出到指定的文件中







<property>
    <name>hive.support.concurrency</name>
    <value>true</value>
  </property>
    <property>
    <name>hive.enforce.bucketing</name>
    <value>true</value>
  </property>
    <property>
    <name>hive.exec.dynamic.partition.mode</name>
    <value>nonstrict</value>
  </property>
  <property>
    <name>hive.txn.manager</name>
    <value>org.apache.hadoop.hive.ql.lockmgr.DbTxnManager</value>
  </property>
    <property>
    <name>hive.compactor.initiator.on</name>
    <value>true</value>
  </property>
  <property>
    <name>hive.compactor.worker.threads</name>
    <value>1</value>
  </property>
  <property>
	<name>hive.in.test</name>
	<value>true</value>
  </property>





