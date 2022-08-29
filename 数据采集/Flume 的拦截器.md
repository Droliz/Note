# Flume 的拦截器

## 准备

安装 JDK 并配置好环境变量
安装配置好 flume 
开启 windows 的 telnet 服务

参考[Flume 的安装配置](https://zhuanlan.zhihu.com/p/511455862)

### 配置采集配置文件

在flume的conf目录下配置一个简单的采集方案配置`timestamp.conf`

```conf
a1.sources = r1  
a1.sinks = k1  
a1.channels = c1

#type是netcat可以从一个网络端口接受数据的。  
#bind绑定本机0.0.0.0,表示可用的所有ip地址

a1.sources.r1.type = netcat  
a1.sources.r1.bind = 0.0.0.0  
a1.sources.r1.port = 444  
a1.sources.r1.interceptors = i1  
a1.sources.r1.interceptors.i1.type = timestamp

#type，下沉类型，使用logger，将数据打印到屏幕上面。  
a1.sinks.k1.type = logger

#capacity：默认该通道中最大的可以存储的event数量，1000是代表1000条数据。  
#trasactionCapacity：每次最大可以从source中拿到或者送到sink中的event数量。  
a1.channels.c1.type = memory  
a1.channels.c1.capacity = 1000  
a1.channels.c1.transactionCapacity = 100

#将sources和sinks绑定到channel上面。  
a1.sources.r1.channels = c1  
a1.sinks.k1.channel = c1
```

![](http://www.drolia.cn/markdown_img/Pasted%20image%2020220518101116.png)

### 测试

开启flume服务

```sh
bin/flume-ng agent --conf conf --conf-file conf/timestamp.conf --name a1 -Dflume.root.logger=INFO,console
```

在windows终端通过telnet连接

```sh
telnet masater 444
```

随后在终端发送的内容可以在flume查看到

![](http://www.drolia.cn/markdown_img/Pasted%20image%2020220518102231.png)

![](http://www.drolia.cn/markdown_img/Pasted%20image%2020220518102243.png)