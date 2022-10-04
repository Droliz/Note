## 准备

在hive中准备数据

创建一个数据库

```sql
create database userdb;

use userdb;

# 创建表：
CREATE TABLE student(id int, name string);

# 插入测试数据：
insert into student(id,name) values(1,'zhangsan');
insert into student(id,name) values(2,'lisi');
insert into student(id,name) values(3,'wangwu');
insert into student(id,name) values(4,'zhaoliu');
insert into student(id,name) values(5,'yangqi');

# 查看数据：
select * from student;

# 操作成功后使用exit命令退出。
exit;
```

查询得到数据

```txt
OK
1       zhangsan
2       lisi
3       wangwu
4       zhaoliu
5       yangqi
Time taken: 0.137 seconds, Fetched: 5 row(s)
```

## 使用spark-sql连接hive

连接hive需要启动hive2服务，hive2服务是专门用于第三方连接的

```sh
hive --service hiveserver2
```

