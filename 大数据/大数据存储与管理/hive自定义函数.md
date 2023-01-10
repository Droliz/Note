---
title: hive自定义函数
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/506884444
---
# hive自定义函数

## 准备

确保mysql允许root远程登录

确保hive的正常运行

## 操作

### 操作现有的函数

#### 查看系统自带函数

```hql
show functions;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220427094925.png)

#### 查看自带函数的用法

```sql
desc functions [extended] FUN_NAME;
```

参数`extended`表示查看详细信息

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220427095218.png)

### 自定义函数UDF编程步骤

* 继承`org.apache.hadoop.hive.ql.UDF`
* 需要实现evaluate函数（evaluate函数支持重载）
* 在hive的命令窗口创建函数

**添加jar**

```sql
add jar JAR_PATH
```
注意，如果是永久函数则必须指定非本地URI，例如HDFS位置。

**创建function**
```sql
create [temporary] function [DB_NAME.]FUN_NAME AS CLASS_NAME [USING JAR|FILE|ARCHIVE 'FILE_URI' [, JAR|FILE|ARCHIVE 'FILE_URI'] ];
```

**删除函数**
```sql
drop [temporary] function [if exists] [DB_NAME.]FUNCTION_NAME;
```
注意：UDF必须要有返回类型，可以返回null，但是返回类型不能为void。

### 导出jar包

#### 创建 maven 工程

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428184355.png)


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428184411.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428184429.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428184511.png)

在此工程中需要创建`com.example.hive.udf`包下创建类（需要更改`Superclass`），此类中包含着源代码

```java
package com.example.hive.udf;

import org.apache.hadoop.hive.ql.exec.UDF;
import org.apache.hadoop.io.Text;

public class Lower extends UDF {
	public Text evaluate(final Text s) {
		if (s == null) {
			return null;
		}
		return new Text(s.toString().toLowerCase());
	}
}
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428190051.png)

在项目的根目录下的`pom.xml`文件中配置如下

```java
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>hiveUdf</groupId>
  <artifactId>hiveUdf</artifactId>
  <version>0.0.1-SNAPSHOT</version>
  <dependencies>
    <dependency>
      <groupId>org.apache.hadoop</groupId>
      <artifactId>hadoop-client</artifactId>
      <version>2.9.2</version>
      <scope>provided</scope>
    </dependency>
    <dependency>
      <groupId>org.apache.hive</groupId>
      <artifactId>hive-exec</artifactId>
      <version>2.3.9</version>
      <scope>provided</scope>
    </dependency>
  </dependencies>
</project>
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428184631.png)

需要修改个人的jdk为自己电脑上的jdk，而不是用自带的1.5

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428184952.png)

最后等待构建完成会在target目录下看到导出的jar包

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428190640.png)

### 构建临时自定义函数

上传到虚拟机上

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428191015.png)

```sql
add jar "YOUR_JAR_PATH"
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428190840.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428190630.png)

删除自定义临时函数

```sql
drop function FUN_NAME;
```

### 构建永久函数
将`jar`包上传到`hdfs`上

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428192513.png)

创建永久函数与jar关联

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428193150.png)

测试

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428193201.png)

删除

```sql
drop function FUN_NAME;
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220428193401.png)