---
title: Phoenix的安装配置（完全分布式）
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/496797201
---
# Phoenix的安装配置（完全分布式）

## 前提准备工作

* 1、[hadoop完全分布式安装](https://zhuanlan.zhihu.com/p/491287061)
* 2、[hbase完全分布式安装](https://zhuanlan.zhihu.com/p/496108543)

将压缩文件解压到/opt目录下

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411102223.png)

## 配置

### 拷贝配置文件

`cp /opt/apache-phoenix-5.0.0-HBase-2.0-bin/phoenix-5.0.0-HBase-2.0-client.jar /opt/hbase-2.1.0/lib/`
`cp /opt/apache-phoenix-5.0.0-HBase-2.0-bin/phoenix-core-5.0.0-HBase-2.0.jar /opt/hbase-2.1.0/lib/`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411102830.png)

分发到从节点上

`scp /opt/apache-phoenix-5.0.0-HBase-2.0-bin/phoenix-5.0.0-HBase-2.0-client.jar root@slave1:/opt/hbase-2.1.0/lib/`
`scp /opt/apache-phoenix-5.0.0-HBase-2.0-bin/phoenix-core-5.0.0-HBase-2.0.jar root@slave1:/opt/hbase-2.1.0/lib/`
`scp /opt/apache-phoenix-5.0.0-HBase-2.0-bin/phoenix-5.0.0-HBase-2.0-client.jar root@slave2:/opt/hbase-2.1.0/lib/`
`scp /opt/apache-phoenix-5.0.0-HBase-2.0-bin/phoenix-core-5.0.0-HBase-2.0.jar root@slave2:/opt/hbase-2.1.0/lib/`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411103326.png)


将hbase的配置文件`hbase-site.xml`拷贝到Phoenix的bin目录下

`cp /opt/hbase-2.1.0/conf/hbase-site.xml /opt/apache-phoenix-5.0.0-HBase-2.0-bin/bin/`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411103741.png)

将Hadoop的配置文件`core-site.xml、hdfs-site.xml`拷贝到Phoenix的bin目录下

`cp /usr/local/hadoop/etc/hadoop/core-site.xml /opt/apache-phoenix-5.0.0-HBase-2.0-bin/bin/`
`cp /usr/local/hadoop/etc/hadoop/hdfs-site.xml /opt/apache-phoenix-5.0.0-HBase-2.0-bin/bin/`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411103922.png)

### 配置环境变量

`vi /etc/profile`

```
export PHOENIX_HOME=/opt/apache-phoenix-5.0.0-HBase-2.0-bin/
export PHOENIX_CLASSPATH=$PHOENIX_HOME
export PATH=$PATH:$PHOENIX_HOME/bin

```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411114644.png)

### 修改权限

`chmod 777 /opt/apache-phoenix-5.0.0-HBase-2.0-bin/bin/psql.py`
``chmod 777 /opt/apache-phoenix-5.0.0-HBase-2.0-bin/bin/sqlline.py``

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411104541.png)

### 分发

`scp -r /opt/apache-phoenix-5.0.0-HBase-2.0-bin/ root@slave1:/opt/`
`scp -r /opt/apache-phoenix-5.0.0-HBase-2.0-bin/ root@slave2:/opt/`

配置从节点的phoenix的环境变量


## 测试
开启hadoop、zookeeper、hbase

如果在配置之前已经开启了，那么需要先关闭hbase再开启

`stop-hbase.sh`无法关闭的话，可以直接使用`kill -9 HMaster进程号`来关闭，然后启动hbase

由于已经配置了环境变量所以可以直接使用命令`sqlline.py master`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411185153.png)

### 测试命令

查看表
```
!tables
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411174538.png)

创建表(表名也可以不加双引号，不加则表名默认转换为大写，如果加上双引号，以后对

表查询时都要加上双引号)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411175205.png)

退出
```
!quit
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411175249.png)