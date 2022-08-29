---
title: Hadoop常用命令
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/496068977
---

# Hadoop常用命令

### 查看hadoop版本

```sh
hadoop version
```

### 查看目录

```sh
# hdfs dfs -ls path
hdfs dfs -ls /	查看根目录的内容

hdfs dfs -ls -R / 递归查看子目录所有内容
```

### 创建目录

```sh
# hdfs dfs -mkdir dir_path_name
hdfs dfs -mkdir /test	# 在根目录 / 下创建 test 目录
# 此时再查看
hdfs dfs -ls /
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220318090517.png)

### 将本地文件拷贝到HDFS上

```sh
# hdfs dfs -put path_file_name hdfs_path
hdfs dfs -put /etc/profile /test

# hdfs dfs -copyFrmLocal path_file_name hdfs_path
# 同put一样的效果
hdfs dfs -copyFrmLocal /etc/profile /test
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220318091658.png)

### 将HDFS文件拷贝到本地上

```sh
# hdfs dfs -get hdfs_path path_file
hdfs dfs get /test/profile /home

# hdfs dfs -getmerge [-nl] <src> <localdest>
# src可以是路径 -nl 可选参数表示再每个文件结尾添加换行
# hdfs dfs -getmerge hdfs_path path_file
hdfs dfs -getmerge -nl /test/profile /home
# 与 get 不同的是，getmerge可以将源路径的多个文件取到本地，同时合并到文件中

# hdfs dfs -copyToLocal hdfs_path path_file
hdfs dfs -copyToLocal /test/profile /home
# 不过与 -copyFromLocal 不同的是后者有 -f 参数可以强制替换已存在的文件
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220318092244.png)

### 显示文件及目录访问控制列表

```sh
# hadoop fs -getfacl path
hadoop fs -getfacl -R /test
# -R 递归显示
# -n name 显示指定属性名的值
# -d 显示指定路径名的所有扩展属性的值
# -e 内容编码（"text"、"hex"、"base64"等双引号括起来）
```

### cat
```sh
# hdfs dfs -cat path_file 	查看指定路径文件
hdfs dfs -cat /test/profile
```

### mv
```sh
# hdfs dfs -mv path_file_1 path_file_2 移动文件位置
hdfs dfs -mv /test/profile /
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220318094039.png)

### cp
```sh
# hdfs dfs -cp path_1 path_2 	拷贝指定路径文件到指定路径
hdfs dfs -cp /test/profile /
```

