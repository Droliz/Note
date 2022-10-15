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

将hive的conf目录下的`hive-site.sh`拷贝到`spark`的conf目录下

将hive的`lib`目录下的mysql驱动拷贝到`soark`的`jars`目录下

**启动spark并打开spark-sql**

```sh
./sbin/start-all.sh

./bin/spark-sql
```

在hive中查看表

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020221010152859.png)

在spark中查看表

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020221010152845.png)

至此，spark-sql中访问的hive 就是外部的hive

查看数据

```spark-sql
select * from student;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020221010153018.png)