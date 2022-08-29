---
title: Phoenix的安装配置（伪分布式）
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/496745281
---
# Phoenix的安装配置（伪分布式）

[Phoenix下载地址](http://phoenix.apache.org)

## 前提准备
### 环境
搭建好hadoop的伪分布式和hbase的伪分布式

[hadoop伪分布式安装配置](https://zhuanlan.zhihu.com/p/491426016)

[hbase伪分布式安装配置](https://zhuanlan.zhihu.com/p/496108716)

### Phoenix安装包
[下载Phoenix](http://phoenix.apache.org)

## 安装Phoenix
>注意:在安装Phoenix之前不要启动hadoop和hbase

通过ssh工具将下载好的Phoenix压缩包上传到虚拟机上，我这里是上传到了home根目录下

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411165627.png)

解压到想要的地方，我这里是解压到`/opt/`下，如果名字太长可以自己更改，我这里改为了phoenix-5.0.0

`tar -zxvf /home/apache-phoenix-5.0.0-HBase-2.0-bin.tar.gz -C /opt/`
`mv /opt/apache-phoenix-5.0.0-HBase-2.0-bin /opt/phoenix-5.0.0`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172052.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172121.png)
## 配置

### 配置环境变量

`vi /etc/profile`

添加如下配置
```
export PHOENIX_HOME=/opt/phoenix-5.0.0/
export PHOENIX_CLASSPATH=$PHOENIX_HOME
export PATH=$PATH:$PHOENIX_HOME/bin
```

`source /etc/profile`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411170733.png)

### 导出文件

将phoenix根目录下的`phoenix-5.0.0-HBase-2.0-client.jar`和`phoenix-core-5.0.0-HBase-2.0.jar`复制到hbase的lib目录下

`cp /opt/phoenix-5.0.0/phoenix-5.0.0-HBase-2.0-client.jar /opt/hbase-2.1.0/lib/`
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172215.png)
`cp /opt/phoenix-5.0.0/phoenix-core-5.0.0-HBase-2.0.jar /opt/hbase-2.1.0/lib/`
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172247.png)
### 导入配置文件

将hbase的配置文件hbase-site.xml导入到phoenix的bin目录下

`cp /opt/hbase-2.1.0/conf/hbase-site.xml /opt/phoenix-5.0.0/bin/`
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172319.png)
将Hadoop的配置文件`core-site.xml、hdfs-site.xml`拷贝到Phoenix的bin目录下

`cp /opt/hadoop-2.9.2/etc/hadoop/core-site.xml /opt/phoenix-5.0.0/bin/`
`cp /opt/hadoop-2.9.2/etc/hadoop/hdfs-site.xml /opt/phoenix-5.0.0/bin/`
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172346.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172420.png)
### 更改文件运行权限
更改phoenix的bin目录下`sqlline.py、psql.py`的权限为777或者给可执行权限`chmod +x`也可

`chmod 777 /opt/phoenix-5.0.0/bin/psql.py`
`chmod 777 /opt/phoenix-5.0.0/bin/sqlline.py`
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172450.png)
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411172518.png)
## 测试
由于已经配置了环境变量所以可以直接使用命令`sqlline.py master`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220411173551.png)

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