# Sqoop安装配置

## 准备

* [hadoop 安装配置](https://zhuanlan.zhihu.com/p/491426016)
* [hbase 安装配置](https://zhuanlan.zhihu.com/p/496108716)
* [hive 安装配置](https://zhuanlan.zhihu.com/p/500689765)
* zookeeper 安装配置（需要复制）


## 安装配置 Sqoop

Sqoop 需要配置环境变量

```sh
export SQOOP_HOME=/opt/sqoop-1.4.7/
export PATH=$PATH:${SQOOP_HOME}/bin
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220528141555.png)

在hdfs下创建目录sqoop用来存储导入的数据

```sh
hdfs dfs -mkdir /sqoop/
```

使用sqoop查看数据库

```sh
bin/sqoop list-databases --connect jdbc:mysql://localhost:3306/ --username USERNAME --password PASSWORD
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220528142100.png)

使用sqoop查看表

```sh
bin/sqoop list-tables --connect jdbc:mysql://localhost:3306/DATABAS_ENAME --username USERNAME --password PASSWORD
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220528142213.png)

导出数据到 hdfs 上

```sh
bin/sqoop import "-Dorg.apache.sqoop.splitter.allow_text_splitter=true" --connect jdbc:mysql://master:3306/DATABASE_NAME --username USERNAME --password PASSWORD --table TABLE_NAME --target-dir HDFS_PATH`
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220528142357.png)

在 hdfs 上查看

```sh
hdfs dfs -cat HDFS_PATH
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220528142435.png)