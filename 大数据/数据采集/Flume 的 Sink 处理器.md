
# Flume 的 Sink 处理器

## 准备

* jdk的安装以及环境变量的配置
* 三台机器的 flume 的 flume-env.sh 的JAVA_HOME 
* 检查三台机器的网络环境


## 配置采集文件

### master 的 sinkgroup.conf

给 k1 的优先级是 10，给 k2 的优先级是 5 ，所有消息会先发给 k1 代表的 slave1 ，如果 slave1 没有开启，才会发给 k2 代表的 slave2

```conf
#a1是agent的代表。  
a1.sources = r1  
a1.sinks = k1 k2  
a1.channels = c1

a1.sources.r1.type = netcat  
a1.sources.r1.bind = master  
a1.sources.r1.port = 444

a1.sinkgroups = g1  
a1.sinkgroups.g1.sinks = k1 k2  

a1.sinks.k1.type = avro  
a1.sinks.k1.hostname=slave1  
a1.sinks.k1.port = 555

a1.sinks.k2.type = avro  
a1.sinks.k2.hostname=slave2  
a1.sinks.k2.port = 555

a1.sinkgroups.g1.processor.type = failover  
a1.sinkgroups.g1.processor.priority.k1 = 10  
a1.sinkgroups.g1.processor.priority.k2 = 5

a1.channels.c1.type = memory  
a1.channels.c1.capacity = 1000  
a1.channels.c1.transactionCapacity = 100

#Bind the source and sink to the channel 描述和配置source channel sink之间的连接关系  
#将sources和sinks绑定到channel上面。  
a1.sources.r1.channels = c1  
a1.sinks.k1.channel = c1  
a1.sinks.k2.channel = c1
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521095417.png)


### slave1 的 sinkgroup1.conf

```conf
a1.sources = r1  
a1.channels = c1  
a1.sinks = k1

a1.sources.r1.type = avro  
a1.sources.r1.bind = 0.0.0.0  
a1.sources.r1.port = 555

a1.sinks.k1.type = logger

a1.channels.c1.type = memory  
a1.channels.c1.capacity = 100  
a1.channels.c1.transactionCapacity = 100

a1.sources.r1.channels = c1  
a1.sinks.k1.channel = c1
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521095619.png)

### slave2 的 sinkgroup2.conf

```conf
a1.sources = r1
a1.channels = c1
a1.sinks = k1

a1.sources.r1.type = avro
a1.sources.r1.bind = 0.0.0.0
a1.sources.r1.port = 555

a1.sinks.k1.type = logger 

a1.channels.c1.type = memory
a1.channels.c1.capacity = 100
a1.channels.c1.transactionCapacity = 100

a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521095706.png)

## 启动 flume 服务

先启动 slave1 和 slave2 的服务

**slave1**

```sh
bin/flume-ng agent --conf conf --conf-file conf/sinkgroup1.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521100915.png)

**slave2**

```sh
bin/flume-ng agent --conf conf --conf-file conf/sinkgroup2.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521101004.png)

**master**

```sh
bin/flume-ng agent --conf conf --conf-file conf/sinkgroup.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521101106.png)

此时，在 slave1 和 slave2 可以看到 master 连接上的信息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521101202.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521101217.png)

## 测试

在 windows 的终端通过 telnet 连接 master 的 444 端口

此时发送消息，可以在slave1看到，但是不会在slave2看到

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521101417.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521101535.png)

关闭slave1 的flume 服务，再次发送消息，此时消息就会在 slave2 显示

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521101622.png)

此时如果重新启动 slave1 ，slave1也不会接收到消息，消息依旧发送给 slave2