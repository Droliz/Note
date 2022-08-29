---
title: 调用Java API（Windows）操作HBase
zhihu-url: https://zhuanlan.zhihu.com/p/496105738
---

# 调用Java API（Windows）操作HBase

## 准备工作

###   配置Windows的hosts

Windows的`C:\Windows\System32\drivers\etc`下的`hosts`文件
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408211153.png)

向文件中添加分布式的主机ip和主机名的映射关系（全部添加）如果权限不够，添加权限
![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408211343.png)

### 准备hadoop和hbase

使用SFTP连接，将虚拟机上的文件传到本机上

从`master`（主节点）拷贝Hadoop的根目录所有文件到主机指定的`hadoop_path`

>由于文件较大，可以选择先压缩再传`tar -zcvf file_path_name Hadoop根目录`

然后压缩hbase的lib目录下的所有文件和conf目录下的hbase-site.xml配置文件

同hadoop

### 配置Windows的Hadoop系统环境变量

#### 添加HADOOP_HOME

在系统环境变量中添加HADOOP_HOME`value   HADOOP_PATH_Windows`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408211807.png)

>注意：这里的Hadoop路径是你Windows上的Hadoop的路径，如果环境变量中没有JAVA_HOME那么一定要添加，因为Hadoop中有些配置用到的是JAVA_HOME，除非自己重新更改Windows中的hadoop配置的JAVA_HOME为Windows的jdk路径，否则手动添加JAVA_HOME

#### 添加HADOOP_USER_NAME

再添加HADOOP_USER_NAME`value   root`

这里的值可以不是root，代表的是用户名可以自己改，但是要保证拥有所有的权限

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408213137.png)

#### 添加path

再path中添加`value   %HADOOP_HOME%\bin`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408213424.png)

### 添加Windows下的运行文件

[下载地址1](https://github.com/kensdm/winutils)
[下载地址2](https://github.com/steveloughran/winutils)

下载Windows的编译后文件，可以选择全部添加到Windows的Hadooop的bin目录下，可以以选择将其中除`winutils.exe、winutils.pdb`文件添加到`C:\Winodws\System32\`目录下

下面是直接将所有文件放到了hadoop的bin目录下

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408220756.png)
## JAVA项目

### 准备eclipse

下载连接[eclipse](https://www.eclipse.org/)

### 创建java项目

进入eclipse点击files，new一个java项目

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408214137.png)

>注意：eclipse是会自带一个jdk的，更改为自己的jdk路径，如果不记得可以查看环境变量中的`JAVA_HOME`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408214311.png)

如果没有，只有jre，那么将自己的jdk添加进去

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408214517.png)

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408214810.png)

先点击configure JRES配置jdk路径，再点击add，然后下一步，将自己的路径添加进去

### 添加配置

将从虚拟机上下载的hbase的lib文件放到java项目的根目录下

将hbase的配置文件hbase-site.xml放到项目的src目录下

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408215225.png)

在src下新建一个日志文件`log4j.properties`，其中rootLogger值为error表示只显示错误信息，也可以是warn和info

```properties
log4j.rootLogger=error,console

log4j.appender.console=org.apache.log4j.ConsoleAppender
log4j.appender.console.layout=org.apache.log4j.PatternLayout
log4j.appender.console.layout.ConversionPattern=%d{yyyy-MM-dd HH:mm:ss} %p %c{1}: %m%n
```

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220410011129.png)

`添加需要的依赖包`：将lib文件根目录下的所有文件和client-facing-thirdparty下的文件全部添加到配置中

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220408215628.png)

### java_api

[API参考文档](https://hbase.apache.org/2.1/devapidocs/index.html)

项目的包创建在src的根目录下，包中创建java的主项目代码

#### 连接hbase

```java
// 创建配置对象
Configuration conf = HBaseConfiguration.create(); 

 // 导入配置文件
conf.addResource("/src/hbase-site.xml");

 // 连接HBase
Connection conn = ConfigurationFactory.createConnection(conf); 

 // 获取Admin对象
Admin admin = conn.getAdmin();
```

### DDL

#### 获取表列表

```java
public void list(Connection conn) throws IOException {
	// 获取Admin管理对象
	Admin admin = conn.getAdmin();
	// 获取表的列表
	TableName[] listTableNames = admin.listTableNames();
	for (TableName tableName : listTableNames) {
		System.out.println(tableName);
	}
	System.out.println("已列出所有表");
}
```

#### 创建表

创建表的时候至少要创建一个列簇

```java
 // 创建表
public void createTable(Connection conn, String tabName, String cf) throws IOException {
	// 获取Admin管理对象
	Admin admin = conn.getAdmin();
	// 构建表名对象
	TableName name = TableName.valueOf(tabName);
	// 构建表描述器
	TableDescriptorBuilder tabDesBuilder = TableDescriptorBuilder.newBuilder(name);
	// 构建列族描述器
	ColumnFamilyDescriptorBuilder cfDesBuilder = ColumnFamilyDescriptorBuilder.newBuilder(Bytes.toBytes(cf));

	// 构建列族描述
	ColumnFamilyDescriptor family = cfDesBuilder.build();
	// 将列族描述添加到表描述器
	tabDesBuilder.setColumnFamily(family);
	// 构建表描述
	TableDescriptor tabDes = tabDesBuilder.build();
	// 创建表
	admin.createTable(tabDes);
	System.out.printf("已成功创建表%s\n", tabName);
 }
```

#### 添加列簇

```java
public void addColumnFamily(Connection conn, String tabName, String cf) throws IOException {
	// 获取Admin管理对象
	Admin admin = conn.getAdmin();
	// 构建表名对象
	TableName name = TableName.valueOf(tabName);

	// 构建列族描述器
	ColumnFamilyDescriptorBuilder cfDesBuilder = ColumnFamilyDescriptorBuilder.newBuilder(Bytes.toBytes(cf));
	// 最大版本为3
	cfDesBuilder.setMaxVersions(3);
	// 构建列族描述
	ColumnFamilyDescriptor family = cfDesBuilder.build();

	// 增加列族
	admin.addColumnFamily(name, family);
	System.out.printf("已成功在%s添加列族%s\n", tabName, cf);
}	
```

#### 修改列簇

```java
public void modifyColumnFamily(Connection conn, String tabName, String cf) throws IOException {
	// 获取Admin管理对象
	Admin admin = conn.getAdmin();
	// 构建表名对象
	TableName name = TableName.valueOf(tabName);

	// 构建列族描述器
	ColumnFamilyDescriptorBuilder cfDesBuilder = ColumnFamilyDescriptorBuilder.newBuilder(Bytes.toBytes(cf));
	// 设置列族属性
	cfDesBuilder.setMaxVersions(3);

	// 构建列族描述
	ColumnFamilyDescriptor family = cfDesBuilder.build();

	// 修改列族
	admin.modifyColumnFamily(name, family);
	System.out.printf("已成功修改%s的列簇\n", tabName);
}
```

#### 删除列簇

```java
public void deleteColumnFamily(Connection conn, String tabName, String cf) throws IOException {
	// 获取Admin管理对象
	Admin admin = conn.getAdmin();
	// 构建表名对象
	TableName name = TableName.valueOf(tabName);

	// 删除列族
	admin.deleteColumnFamily(name, Bytes.toBytes(cf));
	System.out.printf("已删除表%s列簇%s\n", tabName, cf);
}
```

#### 显示表的描述

```java
public void desc(Connection conn, String tabName,String cf) throws IOException {
	// 获取Admin管理对象
	Admin admin = conn.getAdmin();
	// 构建表名对象
	TableName name = TableName.valueOf(tabName);

	// 获取表描述
	TableDescriptor tabDes = admin.getDescriptor(name);
	System.out.printf("表%s的描述\n", tabName);
	System.out.println(tabDes);
	// 获取具体列族描述
	if (cf != null) {
		ColumnFamilyDescriptor columnFamily = tabDes.getColumnFamily(Bytes.toBytes(cf));
		System.out.println(columnFamily);
	}
}
```

#### 清空表

清空表和删除表都是在被禁用的表上进行的，所有需要先禁用表

由于清空表是一个比较危险的操作，所以建议添加确认提示

```java
public void truncateTable(Connection conn, String tabName) throws IOException {
	// 获取Admin管理对象
	Admin admin = conn.getAdmin();
	// 构建表名对象
	TableName name = TableName.valueOf(tabName);

	//清空表之前要先禁用
	admin.disableTable(name);
	// 清空表
	admin.truncateTable(name, true);
	System.out.println("表已清空");
	admin.enableTable(name);
	
}
```

#### 删除表

```java
public void deleteTable(Connection conn, String tabName) throws IOException {
	// 获取Admin管理对象
	Admin admin = conn.getAdmin();
	// 构建表名对象
	TableName name = TableName.valueOf(tabName);
	
	if (! admin.tableExists(name)) {
		System.out.println("要删除的表不存在");
		System.exit(0);
	}
	
	//删除表之前也要先禁用
	admin.disableTable(name);
	// 删除表
	admin.deleteTable(name);
	System.out.println("删除表成功");
	
}
```

### DML

#### 添加数据

```java
public void put(Connection conn, String tabName, List data) throws IOException {
	// 获取表名对象
	TableName name = TableName.valueOf(tabName);
	// 获取表对象
	Table table = conn.getTable(name);
	
	table.put(data);
	System.out.println("添加数据成功!");
}
```

#### 获取数据

```java
public void get(Connection conn, String tabName, String rk) throws IOException {
	// 获取表名对象
	TableName name = TableName.valueOf(tabName);
	// 获取表对象
	Table table = conn.getTable(name);
	Get get = new Get(Bytes.toBytes(rk));
	// 过滤到行的列族的具体列
	get.addColumn(Bytes.toBytes("info"), Bytes.toBytes("name"));
	// 查询get
	Result result = table.get(get);
	System.out.println("查询的数据");
	System.out.println("rowkey" + "\t" + "columnFamliy" + "\t" + "column" + "\t" + "value");
	
	for (Cell cell : result.listCells()) {
		System.out.println(Bytes.toString(CellUtil.cloneRow(cell)) + "\t"
				+ Bytes.toString(CellUtil.cloneFamily(cell)) + "\t\t"
				+ Bytes.toString(CellUtil.cloneQualifier(cell)) + "\t" + Bytes.toString(CellUtil.cloneValue(cell)));
	}
}
```

#### 查看表

```java
public void scan(Connection conn, String tabName) throws IOException {
	// 获取表名对象
	TableName name = TableName.valueOf(tabName);
	// 获取表对象
	Table table = conn.getTable(name);

	Scan scan = new Scan();
	//按列族扫描
//		scan.addFamily(Bytes.toBytes("info"));
	//按具体列扫描
	scan.addColumn(Bytes.toBytes("info"), Bytes.toBytes("name")).withStartRow(Bytes.toBytes("1001")).withStopRow(Bytes.toBytes("1002"), true);
	
	// 查询scan
	ResultScanner scanner = table.getScanner(scan);
	System.out.printf("查看%s\n", table);
	for (Result result : scanner) {

		System.out.println("rowkey" + "\t" + "columnFamliy" + "\t" + "column" + "\t" + "value");
		for (Cell cell : result.listCells()) {
			System.out.println(
					Bytes.toString(CellUtil.cloneRow(cell)) + "\t" + Bytes.toString(CellUtil.cloneFamily(cell))
							+ "\t\t" + Bytes.toString(CellUtil.cloneQualifier(cell)) + "\t"
							+ Bytes.toString(CellUtil.cloneValue(cell)));
		}
	}

}
```

#### 删除

```java
public void delete(Connection conn, String tabName, String rk) throws IOException {
	// 获取表名对象
	TableName name = TableName.valueOf(tabName);
	// 获取表对象
	Table table = conn.getTable(name);
	Delete delete = new Delete(Bytes.toBytes(rk));
	// 删除指定行的列的数据，但只能删除最新版本的数据 Delete
	// delete.addColumn(Bytes.toBytes("other"), Bytes.toBytes("id"));
	// 删除指定行的列的数据的所有版本 DeleteColumn
	// delete.addColumns(Bytes.toBytes("other"), Bytes.toBytes("id"));
	// 删除整个列族 DeleteFamily
	// delete.addFamily(Bytes.toBytes("other"));
	//删除数据 逐个列族DeleteFamily

	table.delete(delete);
	System.out.println("删除数据成功");
}
```

## 整体

### DDL类

```java
package test;

import java.io.IOException;

import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.Admin;
import org.apache.hadoop.hbase.client.ColumnFamilyDescriptor;
import org.apache.hadoop.hbase.client.ColumnFamilyDescriptorBuilder;
import org.apache.hadoop.hbase.client.Connection;
import org.apache.hadoop.hbase.client.TableDescriptor;
import org.apache.hadoop.hbase.client.TableDescriptorBuilder;
import org.apache.hadoop.hbase.util.Bytes;


public class Ddl {

	public void list(Connection conn) throws IOException {
		// 获取Admin管理对象
		Admin admin = conn.getAdmin();
		// 获取表的列表
		TableName[] listTableNames = admin.listTableNames();
		for (TableName tableName : listTableNames) {
			System.out.println(tableName);
		}
		System.out.println("已列出所有表");
	}

	public void createTable(Connection conn, String tabName, String cf) throws IOException {
		// 获取Admin管理对象
		Admin admin = conn.getAdmin();
		// 构建表名对象
		TableName name = TableName.valueOf(tabName);
		// 判断表是否存在
		if (admin.tableExists(name)) {			
			System.out.printf("表%s已存在不能重复创建\n", tabName);
			System.exit(0);
		} else {
			// 构建表描述器
			TableDescriptorBuilder tabDesBuilder = TableDescriptorBuilder.newBuilder(name);
			// 构建列族描述器
			ColumnFamilyDescriptorBuilder cfDesBuilder = ColumnFamilyDescriptorBuilder.newBuilder(Bytes.toBytes(cf));
			
			// 构建列族描述
			ColumnFamilyDescriptor family = cfDesBuilder.build();
			// 将列族描述添加到表描述器
			tabDesBuilder.setColumnFamily(family);
			// 构建表描述
			TableDescriptor tabDes = tabDesBuilder.build();
			// 创建表
			admin.createTable(tabDes);
			System.out.printf("已成功创建表%s\n", tabName);
		}
	}

	public void addColumnFamily(Connection conn, String tabName, String cf) throws IOException {
		// 获取Admin管理对象
		Admin admin = conn.getAdmin();
		// 构建表名对象
		TableName name = TableName.valueOf(tabName);

		// 构建列族描述器
		ColumnFamilyDescriptorBuilder cfDesBuilder = ColumnFamilyDescriptorBuilder.newBuilder(Bytes.toBytes(cf));
		cfDesBuilder.setMaxVersions(3);
		// 构建列族描述
		ColumnFamilyDescriptor family = cfDesBuilder.build();

		// 增加列族
		admin.addColumnFamily(name, family);
		System.out.printf("已成功在%s添加列族%s\n", tabName, cf);
	}	

	public void modifyColumnFamily(Connection conn, String tabName, String cf) throws IOException {
		// 获取Admin管理对象
		Admin admin = conn.getAdmin();
		// 构建表名对象
		TableName name = TableName.valueOf(tabName);

		// 构建列族描述器
		ColumnFamilyDescriptorBuilder cfDesBuilder = ColumnFamilyDescriptorBuilder.newBuilder(Bytes.toBytes(cf));
		// 设置列族属性
		cfDesBuilder.setMaxVersions(3);

		// 构建列族描述
		ColumnFamilyDescriptor family = cfDesBuilder.build();

		// 修改列族
		admin.modifyColumnFamily(name, family);
		System.out.printf("已成功修改%s的列簇\n", tabName);
	}

	public void deleteColumnFamily(Connection conn, String tabName, String cf) throws IOException {
		// 获取Admin管理对象
		Admin admin = conn.getAdmin();
		// 构建表名对象
		TableName name = TableName.valueOf(tabName);

		// 删除列族
		admin.deleteColumnFamily(name, Bytes.toBytes(cf));
		System.out.printf("已删除表%s列簇%s\n", tabName, cf);
	}

	public void desc(Connection conn, String tabName,String cf) throws IOException {
		// 获取Admin管理对象
		Admin admin = conn.getAdmin();
		// 构建表名对象
		TableName name = TableName.valueOf(tabName);

		// 获取表描述
		TableDescriptor tabDes = admin.getDescriptor(name);
		System.out.printf("表%s的描述\n", tabName);
		System.out.println(tabDes);
		// 获取具体列族描述
		if (cf != null) {
			ColumnFamilyDescriptor columnFamily = tabDes.getColumnFamily(Bytes.toBytes(cf));
			System.out.println(columnFamily);
		}
	}
	
	public void truncateTable(Connection conn, String tabName) throws IOException {
		// 获取Admin管理对象
		Admin admin = conn.getAdmin();
		// 构建表名对象
		TableName name = TableName.valueOf(tabName);

		//清空表之前要先禁用
		admin.disableTable(name);
		// 清空表
		admin.truncateTable(name, true);
		System.out.println("表已清空");
		admin.enableTable(name);
		
	}
	
	public void deleteTable(Connection conn, String tabName) throws IOException {
		// 获取Admin管理对象
		Admin admin = conn.getAdmin();
		// 构建表名对象
		TableName name = TableName.valueOf(tabName);
		
		if (! admin.tableExists(name)) {
			System.out.println("要删除的表不存在");
			System.exit(0);
		}
		
		//删除表之前也要先禁用
		admin.disableTable(name);
		// 删除表
		admin.deleteTable(name);
		System.out.println("删除表成功");
		
	}

}
```

### DML类

```java
package test;

import java.io.IOException;
import java.util.List;

import org.apache.hadoop.hbase.Cell;
import org.apache.hadoop.hbase.CellUtil;
import org.apache.hadoop.hbase.TableName;
import org.apache.hadoop.hbase.client.Connection;
import org.apache.hadoop.hbase.client.Delete;
import org.apache.hadoop.hbase.client.Get;
import org.apache.hadoop.hbase.client.Result;
import org.apache.hadoop.hbase.client.ResultScanner;
import org.apache.hadoop.hbase.client.Scan;
import org.apache.hadoop.hbase.client.Table;
import org.apache.hadoop.hbase.util.Bytes;

public class Dml {
	public void put(Connection conn, String tabName, List data) throws IOException {
		// 获取表名对象
		TableName name = TableName.valueOf(tabName);
		// 获取表对象
		Table table = conn.getTable(name);
		
		table.put(data);
		System.out.println("添加数据成功!");
	}

	public void get(Connection conn, String tabName, String rk) throws IOException {
		// 获取表名对象
		TableName name = TableName.valueOf(tabName);
		// 获取表对象
		Table table = conn.getTable(name);

		Get get = new Get(Bytes.toBytes(rk));
		// 过滤到行的列族
//		get.addFamily(Bytes.toBytes("info"));
		// 过滤到行的列族的具体列
		get.addColumn(Bytes.toBytes("info"), Bytes.toBytes("name"));
		// 查询get
		Result result = table.get(get);
		System.out.println("查询的数据");
		System.out.println("rowkey" + "\t" + "columnFamliy" + "\t" + "column" + "\t" + "value");
		
		for (Cell cell : result.listCells()) {
			System.out.println(Bytes.toString(CellUtil.cloneRow(cell)) + "\t"
					+ Bytes.toString(CellUtil.cloneFamily(cell)) + "\t\t"
					+ Bytes.toString(CellUtil.cloneQualifier(cell)) + "\t" + Bytes.toString(CellUtil.cloneValue(cell)));
		}
	}

	public void scan(Connection conn, String tabName) throws IOException {
		// 获取表名对象
		TableName name = TableName.valueOf(tabName);
		// 获取表对象
		Table table = conn.getTable(name);

		Scan scan = new Scan();
		//按列族扫描
//		scan.addFamily(Bytes.toBytes("info"));
		//按具体列扫描
		scan.addColumn(Bytes.toBytes("info"), Bytes.toBytes("name")).withStartRow(Bytes.toBytes("1001")).withStopRow(Bytes.toBytes("1002"), true);
		
		// 查询scan
		ResultScanner scanner = table.getScanner(scan);
		System.out.printf("查看%s\n", table);
		for (Result result : scanner) {

			System.out.println("rowkey" + "\t" + "columnFamliy" + "\t" + "column" + "\t" + "value");
			for (Cell cell : result.listCells()) {
				System.out.println(
						Bytes.toString(CellUtil.cloneRow(cell)) + "\t" + Bytes.toString(CellUtil.cloneFamily(cell))
								+ "\t\t" + Bytes.toString(CellUtil.cloneQualifier(cell)) + "\t"
								+ Bytes.toString(CellUtil.cloneValue(cell)));
			}
		}

	}
	
	public void delete(Connection conn, String tabName, String rk) throws IOException {
		// 获取表名对象
		TableName name = TableName.valueOf(tabName);
		// 获取表对象
		Table table = conn.getTable(name);
		Delete delete = new Delete(Bytes.toBytes(rk));
		// 删除指定行的列的数据，但只能删除最新版本的数据 Delete
		// delete.addColumn(Bytes.toBytes("other"), Bytes.toBytes("id"));
		// 删除指定行的列的数据的所有版本 DeleteColumn
		// delete.addColumns(Bytes.toBytes("other"), Bytes.toBytes("id"));
		// 删除整个列族 DeleteFamily
		// delete.addFamily(Bytes.toBytes("other"));
		//删除数据 逐个列族DeleteFamily
	
		table.delete(delete);
		System.out.println("删除数据成功");
	}
}
```

### main

```java
package test;

import org.apache.hadoop.conf.Configuration;
import org.apache.hadoop.hbase.HBaseConfiguration;
import org.apache.hadoop.hbase.client.Connection;
import org.apache.hadoop.hbase.client.ConnectionFactory;
import org.apache.hadoop.hbase.client.Put;
import org.apache.hadoop.hbase.util.Bytes;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import test.Ddl;
import test.Dml;

class main {
	public static void main(String[] srgs) throws IOException {
		// System.setProperty("hadoop.home.dir", "D:\\soft\\opt\\hadoop-2.9.2");
		Configuration confing = HBaseConfiguration.create();
        confing.addResource("/src/hbase-site.xml");
        Connection conn = ConnectionFactory.createConnection(confing);
        
        // 创建 DDL 对象
        Ddl ddl = new Ddl();
        // 创建 DML 对象
        Dml dml = new Dml();
        
        
        // 查看所有的表
        ddl.list(conn);

        // 创建表
        ddl.createTable(conn, "test_tab", "info");
        
        // 添加列簇
        ddl.addColumnFamily(conn, "test_tab", "other");
        ddl.addColumnFamily(conn, "test_tab", "test_tab_family");
        // 删除列簇
        ddl.deleteColumnFamily(conn, "test_tab", "test_tab_family");
     
        // 查看表描述
        ddl.desc(conn, "test_tab", null);
        
        // 添加数据
        Put put1 = new Put(Bytes.toBytes("1001"));
		put1.addColumn(Bytes.toBytes("info"), Bytes.toBytes("name"), Bytes.toBytes("zhangsan"));
		put1.addColumn(Bytes.toBytes("info"), Bytes.toBytes("age"), Bytes.toBytes("20"));
		put1.addColumn(Bytes.toBytes("info"), Bytes.toBytes("addr"), Bytes.toBytes("beijing"));
		put1.addColumn(Bytes.toBytes("other"), Bytes.toBytes("id"), Bytes.toBytes("1"));
		put1.addColumn(Bytes.toBytes("other"), Bytes.toBytes("email"), Bytes.toBytes("1@1.com"));

		Put put2 = new Put(Bytes.toBytes("1002"));
		put2.addColumn(Bytes.toBytes("info"), Bytes.toBytes("name"), Bytes.toBytes("lisi"));
		put2.addColumn(Bytes.toBytes("info"), Bytes.toBytes("age"), Bytes.toBytes("19"));
		put2.addColumn(Bytes.toBytes("info"), Bytes.toBytes("addr"), Bytes.toBytes("shanghai"));
		put2.addColumn(Bytes.toBytes("other"), Bytes.toBytes("id"), Bytes.toBytes("2"));
		put2.addColumn(Bytes.toBytes("other"), Bytes.toBytes("email"), Bytes.toBytes("21@1.com"));
		
		Put put3 = new Put(Bytes.toBytes("1003"));
		put3.addColumn(Bytes.toBytes("info"), Bytes.toBytes("name"), Bytes.toBytes("wangwu"));
		put3.addColumn(Bytes.toBytes("info"), Bytes.toBytes("age"), Bytes.toBytes("18"));
		put3.addColumn(Bytes.toBytes("info"), Bytes.toBytes("addr"), Bytes.toBytes("guangzhou"));
		put3.addColumn(Bytes.toBytes("other"), Bytes.toBytes("id"), Bytes.toBytes("3"));
		put3.addColumn(Bytes.toBytes("other"), Bytes.toBytes("email"), Bytes.toBytes("31@1.com"));		
        
		List<Put> data = new ArrayList<Put>();
		
		data.add(put1);
		data.add(put2);
		data.add(put3);
		
		dml.put(conn, "test_tab", data);
		
		// 查询数据
		dml.get(conn, "test_tab", "1001");
		dml.get(conn, "test_tab", "1002");
		dml.get(conn, "test_tab", "1003");
		
		// 查看表
		dml.scan(conn, "test_tab");
        
		// 删除数据
		dml.delete(conn, "test_tab", "1003");

		// 查看表
		dml.scan(conn, "test_tab");
		
		ddl.list(conn);

		// 删除表
		ddl.deleteTable(conn, "test_tab");
		
	}
}
```

## 结果

更改前

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220410031416.png)

更改后

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220410031632.png)

代码终端结果

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220410031742.png)