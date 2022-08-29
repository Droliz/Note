---
title: HBase 的安装部署（完全分布式）
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/496108543
---

# HBase 的安装部署（完全分布式）

## 前提

* 1、Hadoop集群工作正常
* 2、集群各节点均设置免密登录

见[完全分布式安装配置](完全分布式安装配置.md)

[zookeeper下载](https://dlcdn.apache.org/zookeeper/zookeeper-3.6.3/)

[HBase下载](https://archive.apache.org/dist/hbase/)

[Apache HBase ™ 参考指南](https://hbase.apache.org/2.1/book.html)

## 安装配置zookeeper

将zookeeper压缩包上传到`/home`下

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330101050.png)

解压到`/opt/`目录下

`tar -zxvf /home/apache-zookeeper-3.6.3-bin.tar.gz -C /opt/`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330101447.png)

创建两个目录

`mkdir /opt/apache-zookeeper-3.6.3-bin/data`

`mkdir /opt/apache-zookeeper-3.6.3-bin/log`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330101533.png)

进入`/opt/apache-zookeeper-3.6.3-bin/conf/`

备份`cp zoo_sample.cfg  zoo.cfg`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330101817.png)

### 修改zoo.cfg

`vi zoo.cfg`

`dataDir=/opt/apache-zookeeper-3.6.3-bin/data`
`dataLogDir=/opt/apache-zookeeper-3.6.3-bin/log`

如果没有这两个参数，自行添加，路径为之前创建的两个目录
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330102403.png)

`cd /opt/apache-zookeeper-3.6.3-bin/data/`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330102835.png)

再次修改zoo.cfg

不同的数字对应不同的机器，每个的根据机器上的`/opt/apache-zookeeper-3.6.3-bin/data/myid`的文件进行不同的配置

```
server:1=master:2888:3888
server:2=slave1:2888:3888
server:3=slave2:2888:3888
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330103220.png)

将zookeeper拷贝到其他的两个节点上

`scp -r /opt/apache-zookeeper-3.6.3-bin/ root@10.244.4.205:/opt/
`
`scp -r /opt/apache-zookeeper-3.6.3-bin/ root@10.244.0.250:/opt/
`

修改两个从节点的`/opt/apache-zookeeper-3.6.3-bin/data/myid`文件

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330104022.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330104032.png)

分别登录三台服务器，进入目录/opt/apache-zookeeper-3.6.3-bin/bin，启动ZK服务: sh zkServer.sh start

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330104259.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330104409.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330104336.png)


查看各个节点的Zookeeper运行状态：  
`sh /opt/apache-zookeeper-3.6.3-bin/bin/zkServer.sh status`
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330104904.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330104917.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330104825.png)

## 安装配置HBase

上传HBase并解压
`tar -zxvf /home/hbase-2.1.0-bin.tar.gz -C /opt/`

编辑：`vi /opt/hbase-2.1.0/conf/hbase-env.sh`文件,修改JAVA_HOME的内容
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330105625.png)

执行 `vi /opt/hbase-2.1.0/conf/regionservers`
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330110111.png)

创建Hbase的临时目录：`mkdir /opt/hbase-2.1.0/tmp`

`cp /opt/hbase-2.1.0/lib/client-facing-thirdparty/htrace-core-3.1.0-incubating.jar /opt/hbase-2.1.0/lib/`

开始编辑：`/opt/hbase-2.1.0/conf/hbase-site.xml` 文件

```xml
<property>  
<name>hbase.regionserver.handler.count</name>  
<value>1000</value>  
</property>  
<property>  
<name>hbase.client.write.buffer</name>  
<value>5242880</value>  
</property>  
<property>  
<name>hbase.rootdir</name>  
<value>hdfs://master:9000/hbase</value>  
</property>  
<property>  
<name>zookeeper.session.timeout</name>  
<value>120000</value>  
</property>  
<property>  
<name>hbase.zookeeper.property.tickTime</name>  
<value>6000</value>  
</property>  
<property>  
<name>hbase.zookeeper.property.dataDir</name>  
<value>/opt/apache-zookeeper-3.6.3-bin</value>  
</property>  
<property>  
<name>hbase.cluster.distributed</name>  
<value>true</value>  
</property>  
<property>  
<name>hbase.zookeeper.quorum</name>  
<value>master,slave1,slave2</value>  
</property>  
<property>  
<name>hbase.tmp.dir</name>  
<value>/opt/hbase-2.1.0/tmp</value>  
</property>  
<property>  
<name>hbase.wal.provider</name>  
<value>filesystem</value>  
</property>  
<property>  
<name>hbase.wal.dir</name>  
<value>hdfs://master:9000/hbase</value>  
</property>  
<property>  
<name>hbase.client.write.buffer</name>  
<value>2097152</value>  
</property>  
<property>  
<name>hbase.regionserver.handler.count</name>  
<value>200</value>  
</property>  
<property>  
<name>hbase.hstore.compaction.min</name>  
<value>6</value>  
</property>  
<property>  
<name>hbase.hregion.memstore.block.multiplier</name>  
<value>16</value>  
</property>  
<property>  
<name>hfile.block.cache.size</name>  
<value>0.2</value>  
</property>  
<property>  
<name>hbase.master</name>  
<value>master:60000</value>  
<description>HMaster</description>  
</property>  
<property>  
<name>hbase.unsafe.stream.capability.enforce</name>  
<value>false</value>  
</property>  
<property>  
<name>hbase.master.distributed.log.splitting</name>  
<value>false</value>  
</property>
```

找到hadoop的安装目录，拷贝core-site.xml文件到hbase的conf目录
`cp /usr/local/hadoop/etc/hadoop/core-site.xml /opt/hbase-2.1.0/conf/`

配置完成的 hbase 目录拷贝到其他各个节点的/opt/下
```
scp -r /opt/hbase-2.1.0/ root@slave1:/opt/
scp -r /opt/hbase-2.1.0/ root@slave2:/opt/
```

在所有节点上的/etc/profile 文件中，完成如下的内容
`vi /etc/profile`
```
export HBASE_HOME=/opt/hbase-2.1.0
export PATH=${HBASE_HOME}/bin:$PATH
```

## 启动

`cd /opt/hbase-2.1.0/bin`
`./start-hbase.sh`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330111626.png)

打开浏览器输入`http://10.244.5.243:16010`检测
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330111751.png)

## 实例

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220330112203.png)




