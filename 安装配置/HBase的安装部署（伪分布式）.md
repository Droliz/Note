---
title: HBase的安装部署（伪分布式）
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/496108716
---
# HBase的安装部署（伪分布式）

## 前提准备

### Hadoop运行正常

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401134845.png)

### 配置hosts

`vi /etc/hosts`

格式：ip 主机名

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401135224.png)

### 关闭防火墙

```sh
systemctl stop firewalld
systemctl disable firewalld
systemctl status firewalld
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401135557.png)

### 配置ssh免密
见[伪分布式的安装配置](伪分布式的安装配置.md)

## 配置
进入存放配置文件的目录
`cd /opt/hbase-2.1.0/conf/`

### hbase-env.sh

```sh
export JAVA_HOME=/opt/jdk1.8.0_112
export HBASE_MANAGES_ZK=true

# 如果JDK1.8需要注释掉以下部分：
# exportHBASE_MASTER_OPTShttp://www.droliz.cnhttp://www.droliz.cn.
# exportHBASE_REGIONSERVER_OPTShttp://www.droliz.cnhttp://www.droliz.cn.
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401142730.png)

### hbase-site.xml

```xml
<property>
<name>hbase.rootdir</name>
<value>hdfs://master:9000/hbase</value>
</property>
<property>
<name>hbase.cluster.distributed</name>
<value>true</value>
</property>
<!--0.98后的变动，之前版本没有.port,默认端口为60000-->
<property>
<name>hbase.master.port</name>
<value>16000</value>
</property>
<property>
<name>hbase.zookeeper.quorum</name>
<value>master:2181</value>
</property>
<property>
<name>hbase.unsafe.stream.capability.enforce</name>
<value>false</value>
</property>
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401142836.png)

###   regionservers
根据自己主机名修改

```
master
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401143005.png)

### 软连接
软连接Hadoop的两个配置文件`core-site.xml和hdfs-site.xml`到hbase的配置文件中

```sh
ln -s /opt/hadoop-2.9.2/etc/hadoop/core-site.xml /opt/hbase-2.1.0/conf/core-site.xml
ln -s /opt/hadoop-2.9.2/etc/hadoop/hdfs-site.xml /opt/hbase-2.1.0/conf/hdfs-site.xml
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401143241.png)

### 拷贝第三方包

`cp /opt/hbase-2.1.0/lib/client-facing-thirdparty/* /opt/hbase-2.1.0/lib/`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401143350.png)

## 启动Hbase
`cd /opt/hbase-2.1.0`
启动
`bin/start-hbase.sh`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401143600.png)

查看进程`jps`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220401143611.png)

其中HBase的两个进程为HMaster、HRegionServer；HQuorumPeer则为内置的Zookeeper进程