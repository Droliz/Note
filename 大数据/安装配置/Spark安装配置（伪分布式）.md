
## 准备

上传

![](../../markdown_img/Pasted%20image%2020220927082520.png)

解压

![](../../markdown_img/Pasted%20image%2020220927082908.png)

## 配置

### spark-env.sh

```sh
export SCALA_HOME=/opt/scala-2.4.7
export JAVA_HOME=/opt/java/jdk
export HADOOP_HOME=/opt/hadoop-2.9.2
export HADOOP_CONF_DIR=/opt/hadoop-2.9.2/etc/hadoop
export SPARK_MASTER_IP=master
export SPARK_MASTER_PORT=7077
```

### profile

```txt
export SPARK_HOME=/opt/spark-2.4.5
export PATH=$PATH:$SPARK_HOME/bin
```

## 启动

进入spark的`sbin`目录

```sh
./start-all.sh
```

![](../../markdown_img/Pasted%20image%2020220927083813.png)

进入`master:8080`查看

![](../../markdown_img/Pasted%20image%2020220927083922.png)


## 整合hive

启动spark-shell导入hive包`import org.apache.spark.sql.hive.HiveContext`

![](../../markdown_img/Pasted%20image%2020221007172836.png)

将hive的lib目录下的mysql的驱动copy到spark的jars目录下

![](../../markdown_img/Pasted%20image%2020221007172958.png)

将

1：将hive-site.xml拷贝到spark/conf里：

`cp /opt/apache-hive-2.3.3-bin/conf/hive-site.xml /opt/spark/conf cd /opt/spark/conf vi hive-site.xml`

3：在hvie-site.xml中将hive.metastore.schema.verification参数设置为false （如果没有该节点则添加以下内容）：

```xml
<property>     
	<name>hive.metastore.schema.verification</name>     
	<value>false</value> 
</property>
```

```sh
cd /opt/spark/sbin
#如果已经启动就先停止spark
./stop-all.sh
#开始启动spark集群
./start-all.sh

cd /opt/spark/bin
./spark-sql
```

通过spark-sql创建数据库：

`show databases;`


通过spark-sql查看已经创建好的数据表:

`use userdb; show tables;`

通过spark-sql查看数据表的数据：

通过spark-sql操作hive的数据：

```sql
select * from student where id>3;  
select count(*) from student; 
select * from student where name like '%a%'; 

#退出命令使用： exit
```
