# Flume 运行监控

## 准备

* 安装部署flume
* jdk的安装和环境变量的配置
* 开启windows的telnet服务

## 配置

### 添加采集配置文件  netcat-logger.conf

```conf
#定义这个agent中各组件的名字，给那三个组件sources，sinks，channels取个名字,是一个逻辑代号:  
#a1是agent的代表。  
a1.sources = r1  
a1.sinks = k1  
a1.channels = c1

#type是类型，是采集源的具体实现，这里是接受网络端口的，netcat可以从一个网络端口接受数据的。  
a1.sources.r1.type = netcat  
a1.sources.r1.bind = 0.0.0.0  
a1.sources.r1.port = 444

#Describe the sink 描述和配置sink组件：k1  
#type，下沉类型，使用logger，将数据打印到屏幕上面。  
a1.sinks.k1.type = logger

#Use a channel which buffers events in memory 描述和配置channel组件，此处使用是内存缓存的方式  
#type类型是内存memory。  
#下沉的时候是一批一批的, 下沉的时候是一个个eventChannel参数解释：  
#capacity：默认该通道中最大的可以存储的event数量，1000是代表1000条数据。  
#trasactionCapacity：每次最大可以从source中拿到或者送到sink中的event数量。  
a1.channels.c1.type = memory  
a1.channels.c1.capacity = 1000  
a1.channels.c1.transactionCapacity = 100

#将sources和sinks绑定到channel上面。  
a1.sources.r1.channels = c1  
a1.sinks.k1.channel = c1
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521102544.png)

## 启动flume服务

```sh
bin/flume-ng agent --conf conf --conf-file conf/netcat-logger.conf --name a1 -Dflume.root.logger=INFO,console -Dflume.monitoring.type=http -Dflume.monitoring.port=1234
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521103002.png)

## 测试

使用windows终端telnet连接master 的444端口

发送消息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521103127.png)

在浏览器`IP:1234/metrics`就可以查看到 flume 的监控信息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521103342.png)

关闭服务，在`flume-env.sh`文件中添加如下配置

```sh
JAVA_OPTS="-Dcom.sun.management.jmxremote  
-Dcom.sun.management.jmxremote.authenticate=false  
-Dcom.sun.management.jmxremote.ssl=false  
-Dcom.sun.management.jmxremote.port=54321  
-Dcom.sun.management.jmxremote.rmi.port=54322  
-Djava.rmi.server.hostname=IP"
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521104651.png)

启动服务

```sh
bin/flume-ng agent --conf conf --conf-file conf/netcat-logger.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521103842.png)

再次通过telnet发送消息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521104749.png)


在windows终端中通过命令`jconsole`启动java监控（如果没有配置环境变量，需要在jdk的bin目录下启动）

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521104124.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521104313.png)

可以查看到监控

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220521104913.png)