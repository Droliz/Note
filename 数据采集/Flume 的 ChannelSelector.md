# Flume 的 ChannelSelector

## 准备

* jdk的安装以及环境变量的配置
* 三台机器的 flume 的安装配置
* 检查三台主机的网络环境
* 开启 windows 的 telnet 服务

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521091037.png)

## 配置

确认配置好三台机器的flume的conf目录下的`flume-env.sh`文件的JAVA_HOME

### 采集配置文件

#### 添加 master 采集配置 replicating.conf

在master的conf目录下添加如下采集配置文件`replicating.conf`

```conf
#Name the components on this agent  
a1.sources = r1  
a1.sinks = k1 k2  
a1.channels = c1 c2

a1.sources.r1.type = netcat  
a1.sources.r1.bind = master  
a1.sources.r1.port = 444  
a1.sources.r1.channels=c1 c2  
a1.sources.r1.selector.type = replicating

a1.sinks.k1.type = avro  
a1.sinks.k1.channel = c1  
a1.sinks.k1.hostname=slave1  
a1.sinks.k1.port = 555

a1.sinks.k2.type = avro  
a1.sinks.k2.channel = c2  
a1.sinks.k2.hostname=slave2  
a1.sinks.k2.port = 555

a1.channels.c1.type = memory  
a1.channels.c1.capacity = 1000  
a1.channels.c1.transactionCapacity = 100  
a1.channels.c2.type = memory  
a1.channels.c2.capacity = 1000  
a1.channels.c2.transactionCapacity = 100

#a1.sources.r1.channels = c1 c2
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521091946.png)

#### 添加 slave1 采集配置 selector1.conf

```conf
a1.sources = r1  
a1.channels = c1  
a1.sinks = k1

a1.sources.r1.type = avro  
a1.sources.r1.bind = slave1  
a1.sources.r1.port = 555

a1.sinks.k1.type = logger

a1.channels.c1.type = memory  
a1.channels.c1.capacity = 100  
a1.channels.c1.transactionCapacity = 100

a1.sources.r1.channels = c1  
a1.sinks.k1.channel = c1
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521092347.png)

#### 添加 slave2 采集配置 selector2.conf

```conf
a1.sources = r1
a1.channels = c1
a1.sinks = k1

a1.sources.r1.type = avro
a1.sources.r1.bind = slave2
a1.sources.r1.port = 555
  
a1.sinks.k1.type = logger

a1.channels.c1.type = memory
a1.channels.c1.capacity = 100
a1.channels.c1.transactionCapacity = 100

a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521092458.png)


## 启动服务

先后启动 slave1、slave2、master 节点的 flume

**slave1**

```sh
bin/flume-ng agent --conf conf --conf-file conf/selector1.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521093120.png)

**slave2**

```sh
bin/flume-ng agent --conf conf --conf-file conf/selector2.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521093145.png)

**master**

```sh
bin/flume-ng agent --conf conf --conf-file conf/replicating.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521093218.png)

当 master 开启 flume 时，可以发现在 slave1 和 slave2 都显示连接成功

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521093339.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521093349.png)

## 测试

使用 winodws 的终端 telnet 连接 master 节点的 444 端口

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521093526.png)

从终端发送的消息，可以在 slave1 和 slave2 节点查看到接收到的消息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521093723.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521093734.png)