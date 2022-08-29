	# Flume整合kafka

## 准备

JDK安装配置（配置环境变量）

## 安装配置kafka

### 安装kafka

 将kafka压缩包解压到 `/opt` 目录下

```sh
var -zxvf kafka_2.11-2.0.0.tar.gz -C /opt/
```

### 配置

#### 配置 zoopkeeper.properties

```
1：创建zookeeper目录：mkdir  /opt/kafka_2.10-0.10.0.1/zookeeper  

2：创建zookeeper日志目录: mkdir /opt/kafka_2.10-0.10.0.1/log/zookeeper
```

```sh
vi /opt/kafka_2.11-2.0.0/confing/zoopkeeper.properties
```

更改如下配置，如果没有，自己添加

```properties
dataDir=/opt/kafka_2.11-2.0.0/zookeeper  #zookeeper数据目录

dataLogDir=/opt/kafka_2.11-2.0.0/log/zookeeper  #zookeeper日志目录

clientPort=2181

maxClientCnxns=100

tickTime=2000

initLimit=10

syncLimit=5
```

#### 配置 server.properties

```sh
vi /opt/kafka_2.11-2.0.0/confing/server.properties
```

```properties
broker.id=1

port=9092 #端口号

host.name=master #服务器IP地址，修改为自己的服务器IP
```

## 测试 kafka
### 启动kafka

```sh
/opt/kafka_2.11-2.0.0/bin/zookeeper-server-start.sh -daemon config/zookeeper.properties
```

这个启动之后用jps查看进程会看到 `QuorumPeerMain` 进程

```sh
/opt/kafka_2.11-2.0.0/bin/kafka-server-start.sh -daemon config/server.properties
```

这个启动之后会看到 `kafka` 进程

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220516101954.png)

### 查看topic

查看已有的topic：

```sh
/opt/kafka_2.11-2.0.0/bin/kafka-topics.sh --list --zookeeper localhost:2181
```

创建名为 test 和 flume 的 topic

```sh
/opt/kafka_2.11-2.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic test  
/opt/kafka_2.11-2.0.0/bin/kafka-topics.sh --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic flume
```

### 测试
测试消息生产者

```sh
/opt/kafka_2.11-2.0.0/bin/kafka-console-producer.sh --broker-list master:9092 --topic test
```

重新开启终端连接master，测试消息消费者

```sh
/opt/kafka_2.11-2.0.0/bin/kafka-console-consumer.sh --bootstrap-server master:9092 --topic test --from-beginning
```

从生产者发送的数据，可以从消费者看到

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220516100014.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220516095958.png)

## 安装配置 flume

参考[flume的安装](https://zhuanlan.zhihu.com/p/511455862) 

## 整合

### 编写 flume-kafka.conf 

编写 flume 的配置文件，在conf目录下

```conf
a1.sources = r1  
a1.sinks = k1  
a1.channels = c1

a1.sources.r1.type = netcat  
a1.sources.r1.bind = master  
a1.sources.r1.port = 44444

#type，下沉类型，使用logger，将数据打印到屏幕上面。  
a1.sinks.k1.type = org.apache.flume.sink.kafka.KafkaSink  
a1.sinks.k1.topic =flume  
a1.sinks.k1.brokerList = master:9092,

#capacity：默认该通道中最大的可以存储的event数量，1000是代表1000条数据。  
#trasactionCapacity：每次最大可以从source中拿到或者送到sink中的event数量。  
a1.channels.c1.type = memory  
a1.channels.c1.capacity = 1000  
a1.channels.c1.transactionCapacity = 100

#将sources和sinks绑定到channel上面。  
a1.sources.r1.channels = c1  
a1.sinks.k1.channel = c1
```

启动 flume

```sh
/opt/flume-1.8.0/bin/flume-ng agent --conf conf --conf-file conf/flume-kafka.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220516104201.png)

在 windows终端连接 flume （需要开启 telnet 客户端）

```sh
telnet master 44444
```

发送消息，可以发现在windows终端向flume发送的消息，可以在kafka的消费者窗口的flume查看

```sh
/opt/kafka_2.11-2.0.0/bin/kafka-console-consumer.sh --bootstrap-server master:9092 --topic flume --from-beginning
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220516104512.png)
