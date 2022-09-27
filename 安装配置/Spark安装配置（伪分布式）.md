
## 准备

上传

![](../markdown_img/Pasted%20image%2020220927082520.png)

解压

![](../markdown_img/Pasted%20image%2020220927082908.png)

## 配置

### spark-env.sh

```sh
export SCALA_HOME=/opt/scala-2.4.5
export JAVA_HOME=/opt/java/jdk
export HADOOP_HOME=/opt/hadoop-2.9.2
export HADOOP_CONF_DIR=/opt/hadoop-2.9.2/etc/hadoop
export SPARK_MASTER_IP=master
export SPARK_MASTER_PORT=7077
```

### profile

```txt
export SPARK_HOME=/opt/spark-2.4.5
export PATH=$PATH:$SPARK_HOME/bin
```

## 启动

进入spark的`sbin`目录

```sh
./start-all.sh
```

![](../markdown_img/Pasted%20image%2020220927083813.png)

进入`master:8080`查看

![](../markdown_img/Pasted%20image%2020220927083922.png)