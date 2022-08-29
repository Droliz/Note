---
title: Flume的安装
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/511455862
---
# Flume的安装

## 准备

* 安装jdk，配置环境变量
* 下载Flume：[下载地址](https://archive.apache.org/)

## 安装配置

### 安装

使用scp命令将下载好的文件上传到虚拟机上

```sh
scp WINDOWS_PATH USER_NAME@HOST_NAME:LINUX_PATH
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509100128.png)

将上传的文件解压到`opt`目录下并更改文件名为`flume-版本`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509100415.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509100446.png)

### 配置

#### flume-env.sh

将flume的conf目录下的`flume-env.sh.template`复制并改名为`flume-env.sh`

```sh
cp flume-env.sh.template flume-env.sh
```

更改`flume-env.sh`中的jdk路径

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509101129.png)

#### netcat-logger.conf

配置采集方案，可以现在主机上配置，然后上传到虚拟机的 `flume` 的 `conf` 目录下

```conf
# 定义这个agent中各组件的名字，给那三个组件sources，sinks，channels取个名字,是一个逻辑代号:
# a1是agent的代表。
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# type是类型，是采集源的具体实现，这里是接受网络端口的，netcat可以从一个网络端口接受数据的。
# bind绑定本机IP（如果配置了hosts映射，那么可以填主机名）。port端口号为444。
a1.sources.r1.type = netcat
a1.sources.r1.bind = master
a1.sources.r1.port = 444
  
# type，下沉类型，使用logger，将数据打印到屏幕上面。

a1.sinks.k1.type = logger

# type类型是内存memory。
# capacity：默认该通道中最大的可以存储的event数量，1000是代表1000条数据。
# trasactionCapacity：每次最大可以从source中拿到或者送到sink中的event数量。
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# 将sources和sinks绑定到channel上面。
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509103951.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509102245.png)

## 利用 windows 的telnet功能测试

开启Windows的telent功能

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509102440.png)
在flume的根目录下运行

```sh
bin/flume-ng agent --conf conf --conf-file conf/netcat-logger.conf --name a1 -Dflume.root.logger=INFO,console
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509104217.png)

在Windows的终端连接服务器

```sh
# 配置了hosts可以使用主机名代替ip
telnet IP PORT
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509103407.png)

然后从终端发送数据，可以在服务器看到发送的东西

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220509103230.png)