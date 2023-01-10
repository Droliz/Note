---
title: Hive的安装配置（伪分布式）
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/500689765
---
# Hive的安装配置（伪分布式）

## 准备

准备hadoop

* [hadoop的完全分布式安装配置](https://zhuanlan.zhihu.com/p/491287061)
* [hadoop的伪分布式安装配置](https://zhuanlan.zhihu.com/p/491426016)

## 安装元数据库（mysql）

```sh
cd /opt/software

wget http://dev.mysql.com/get/mysql57-community-release-el7-10.noarch.rpm
```

如果显示`wget`未找到可能是未安装需要使用yum安装`yum -y install wget`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418103046.png)

```sh
# 使用yum安装
yum -y install mysql57-community-release-el7-10.noarch.rpm
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418103435.png)

安装好mysql之后就可以进行mysql服务器的安装

在显示`error：源 "MySQL 5.7 Community Server" 的 GPG 密钥已安装，但是不适用于此软件包。请检查源的公钥 URL 是否配置正确。`时添加--nogpgcheck参数可以跳过GPG验证

```sh
yum -y install mysql-community-server --nogpgcheck
```


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418103747.png)

启动MySQL

```sh
# 启动mysql服务
systemctl start mysqld
# 查看mysql状态
service mysqld status
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418105015.png)

MySQL5.7 会在安装后为 root 用户生成一个随机密码，而不是像以往版本的空密码。可以在安全模式修改 root 登录密码或者用随机密码登录修改密码。MySQL 为 root 用户生成的随机密码通过 mysqld.log 文件可以查找到：

```sh
cat /var/log/mysqld.log | grep password
# 登录mysql
mysql -u root -p
# 输入刚刚查看到的随机生成的密码
```

`ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.`需要先用`ALTER USER;`修改密码

```
ALTER USER user-name IDENTIFIED BY new-password
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418113322.png)

`ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

这里如果密码设置太简单可能不通过，可以查看密码策略`SHOW VARIABLES LIKE 'validate_password%'`根据自己的需求更改

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418113640.png)

设置root远程登录

```sql
# 先进入mysql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'YOUR_PASSWORD' WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

开启防火墙的3306端口（或者直接关闭防火墙）

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418114336.png)

设置mysql相关参数

`vi /etc/my.cnf`

```cnf
lower_case_table_names=1
character_set_server=utf8
init_connect='SET NAMES utf8'
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418114759.png)

重启MySQL服务

`systemctl restart mysqld`

创建 Hive 的元数据库

`create database metastore;`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418115310.png)


## Hive安装

[Hive安装](https://dlcdn.apache.org/hive/hive-2.3.9/)

安装到指定目录，我这里是下载的安装包，解压到`/opt`下重命名为`hive-版本号`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418124731.png)

### 配置Hive

#### proflie

```
export HIVE_HOME=/opt/hive-2.3.9
export PATH=${HIVE_HOME}/bin:$PATH
```

#### hive-env.sh
先根据配置模板生成配置文件`hive-env.sh`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418125117.png)

编辑配置文件`hive-env.sh`的`HADOOP_HOME`和`HIVE_CONF_DIR`

```sh
export HADOOP_HOME=/opt/hadoop-2.9.2/
export HIVE_CONF_DIR=/opt/hive-2.3.9/conf/
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418125737.png)


## 配置连接元数据库的驱动

下载驱动`wget -c https://downloads.mysql.com/archives/get/p/3/file/mysql-connector-java-5.1.49.tar.gz
`
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418130530.png)

解压，并将解压的文件夹下的`mysql-connector-java-5.1.49.jar`文件拷贝到hive的lib目录下

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418130651.png)

### 配置hive-site.xml

`vi /opt/hive-2.3.9/conf/hive-site.xml`

注意根据自己的配置更改主机名和数据库用户名和密码

```xml
<?xml version="1.0"?>

<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>

<configuration>

<!-- jdbc 连接的 URL -->

<property>

<name>javax.jdo.option.ConnectionURL</name>

<value>jdbc:mysql://YOUR_HOST_NAME:3306/metastore?useSSL=false</value>

</property>

<!-- jdbc 连接的 Driver-->

<property>

<name>javax.jdo.option.ConnectionDriverName</name>

<value>com.mysql.jdbc.Driver</value>

</property>

<!-- jdbc 连接的 username-->

<property>

<name>javax.jdo.option.ConnectionUserName</name>

<value>YOUR_USER</value>

</property>

<!-- jdbc 连接的 password -->

<property>

<name>javax.jdo.option.ConnectionPassword</name>

<value>YOUR_PASSWORD</value>

</property>

<!-- Hive 元数据存储版本的验证 -->

<property>

<name>hive.metastore.schema.verification</name>

<value>false</value>

</property>

<!--元数据存储授权-->

<property>

<name>hive.metastore.event.db.notification.api.auth</name>

<value>false</value>

</property>

<!-- Hive 默认在 HDFS 的工作目录 -->

<property>

<name>hive.metastore.warehouse.dir</name>

<value>/user/hive/warehouse</value>

</property>

<!-- 指定 hiveserver2 连接的 host -->

<property>

<name>hive.server2.thrift.bind.host</name>4

<value>YOUR_HOST_NAME</value>

</property>

<!-- 指定 hiveserver2 连接的端口号 -->

<property>

<name>hive.server2.thrift.port</name>

<value>10000</value>

</property>

<!-- 指定本地模式执行任务，提高性能 -->

<property>

<name>hive.exec.mode.local.auto</name>

<value>true</value>

</property>

</configuration>
```

## 初始化 hive 元数据库

`只需要初始化一次`

```sh
schematool -initSchema -dbType mysql -verbose
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418131755.png)

## 启动 hive

首先保证hadoop启动正常（正常情况下这两个文件夹是会自动生成的）

```
hadoop fs -mkdir /tmp
hadoop fs -mkdir -p /user/hive/warehouse
hadoop fs -chmod g+w /tmp
hadoop fs -chmod g+w /user/hive/warehouse
```

`hive`进入

测试命令

`show databases;`
`show tables;`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418134358.png)

## 启动 Hiveserver2

`hive --service hiveserver2`

前台运行hiveserver2，会显示在前台，如果需要在后台显示加上`&`或使用`hiveserver2 &`命令

启动完成后，可以通过浏览器访问10002端口查看

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418160850.png)

## 启动 beeline 客户端

Beeline：配合Hiveserver2的客户端，更加安全并且不会直接暴露 hdfs 和 metastore

配置hadoop的`core-site.xml`

```xml
<!-- 表示设置 hadoop 的代理用户-->

<property>

<!--表示代理用户的组所属-->6

<name>hadoop.proxyuser.root.groups</name>

<value>*</value>

</property>

<!--表示任意节点使用 hadoop 集群的代理用户 hadoop 都能访问 hdfs 集群-->

<property>

<name>hadoop.proxyuser.root.hosts</name>

<value>*</value>

</property>
```

在beeline控制台 `!connect jdbc:hive2://master:10000`

也可以在该节点的控制台 `beeline -u jdbc:hive2://master:10000`

如果不配置直接访问，会出现如下错误，这是因为hadoop 引入了一个安全伪装机制，使得 hadoop 不允许上层系统直接将实际用户传递到 hadoop 层，而是将实际用户传递给一个超级代理，由此代理在 hadoop 上执行操作，避免任意客户端随意操作 hadoop。

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418161848.png)

username和password是前面设置的数据库的密码

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220418170911.png)