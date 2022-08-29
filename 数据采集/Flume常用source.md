# Flume常用source

## 准备

* 准备jdk，在1.8版本及以上
* 配置好主机和ip地址的映射（hosts）
* 确保两台机器可以ping通

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220511103111.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513082017.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513082036.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513082412.png)

## 配置

### 配置master节点的flume

将压缩包压缩到opt目录下，并更改目录名

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220511103455.png)

### 配置`flume-env.sh`

```sh
cd /opt/flume-1.8.0/conf/
cp flume-env.sh.template flume-env.sh
```

更改JAVA_HOME

```sh
export JAVA_HOME=YOUR_JAVA_HOME
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220511103934.png)

配置`flume_flume_1.conf`文件

```conf
a1.sources = r1  
a1.channels = c1  
a1.sinks = k1

a1.sources.r1.type = netcat  
a1.sources.r1.bind = master  
a1.sources.r1.port = 444

a1.sinks.k1.type = avro  
a1.sinks.k1.channel = c1  
a1.sinks.k1.hostname=slave1  
a1.sinks.k1.port = 555

a1.channels.c1.type = memory  
a1.channels.c1.capacity = 100  
a1.channels.c1.transactionCapacity = 100

a1.sources.r1.channels = c1  
a1.sinks.k1.channel = c1
```

### 配置slave1

同master一样配置`flume-env.sh`文件的JAVA_HOME

由于两个主机的开放端口等不一样，所以`flume_flume_2.conf`配置文件内容也会有所不同

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

先开启slave1的flume再开启master的flume

```sh
// slave1
bin/flume-ng agent --conf conf --conf-file conf/flume_flume_2.conf --name a1 -Dflume.root.logger=INFO,console

// master
bin/flume-ng agent --conf conf --conf-file conf/flume_flume_1.conf --name a1 -Dflume.root.logger=INFO,console
```

**slave1**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513084110.png)

**master**

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220513084128.png)

## 打开Telnet

参考[Flume的安装](https://zhuanlan.zhihu.com/p/511455862)

最后可以看到终端发送的可以slave1中看到


