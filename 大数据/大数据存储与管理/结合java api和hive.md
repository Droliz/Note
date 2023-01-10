---
title: 结合java api和hive
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/507507243
---
# 结合java api和hive

调用java api操作hive可以和[调用Java API（Windows）操作HBase](https://zhuanlan.zhihu.com/p/496105738)一样导入相关的配置文件，也可以通过maven工程实现

## 准备

在主机上准备eclipse和jdk并配置好jdk的环境变量

配置主机上的hosts文件，并使用ping虚拟机的主机名的方式检验主机与虚拟机之间的连通性


创建maven工程在`pom.xml`文件中`dependencies`标签下添加如下配置属性

```xml
<dependency>
	<groupId>org.apache.hive</groupId>
	<artifactId>hive-jdbc</artifactId>
	<version>2.3.9</version>
</dependency>

<dependency>
	<groupId>org.apache.hadoop</groupId>
	<artifactId>hadoop-common</artifactId>
	<version>2.9.2</version>
</dependency>
```

如果此时`pom.xml`第一行报错`Error:Missing artifact jdk.tools:jdk.tools:jar:1.7`那么可以在属性中添加如下依赖，解决

```xml
<dependency>  
	<groupId>jdk.tools</groupId>  
	<artifactId>jdk.tools</artifactId>  
	<version>1.7</version>  
	<scope>system</scope>  
	<systemPath>${JAVA_HOME}/lib/tools.jar</systemPath>  
</dependency> 	
```

## 操作

### 连接hive

远程连接hive必须要开启hive2

后台运行hive2命令：`hiveserver2 &`

### 查看数据库

```java
// 查看数据库
public void showDatabases(Connection con) {
	String sql = "show databases";
	try {
		PreparedStatement pst = con.prepareStatement(sql);
		ResultSet res = pst.executeQuery();
		System.out.println("现有的数据库");
		int i = 1;
		while (res.next()) {
			System.out.println(i + "\t" + res.getString(1));
			i += 1;
		}
	} catch (Exception e) {
		System.out.println(e);
	}
}
```

### 查看表

```java
// 查看表
public void showTables(Connection con, String dbName) {
	
	String sql_1 = "use " + dbName;
	String sql = "show tables";
	try {
		if (dbName != null) {
			PreparedStatement pst_1 = con.prepareStatement(sql_1);
			pst_1.execute();
		}
		PreparedStatement pst = con.prepareStatement(sql);
		ResultSet res = pst.executeQuery();
		int i = 1;
		System.out.println("数据库" + dbName + "的所有表");
		while (res.next()) {
			System.out.println(i + "\t" + res.getString(1));
			i++;
		}
	} catch (Exception e) {
		// TODO: handle exception
		System.out.println(e);
	}
}
```

### 创建表

```java
// 创建表
public void createTable(Connection con, String tableName, String dbName) {
	if (dbName != null) {
		dbName += ".";
	}else {
		dbName = "";
	}
	String  sql = "create table " + dbName + tableName + 
			"(id int, name string)";
	try {
		PreparedStatement pst = con.prepareStatement(sql);
		pst.executeUpdate();
		System.out.println("表" + tableName + "创建成功");
	} catch (Exception e) {
		System.out.println(e);
	}
}
```

### 添加数据

```java
// 添加数据
public void insertData(Connection con, String tableName, String dbName, String id, String name) {
	if (dbName != null) {
		dbName += ".";
	}else {
		dbName = "";
	}
	String sql = "insert into " + dbName + tableName + " values(" + id + ",'" + name + "')";
	
	try {
		PreparedStatement pst = con.prepareStatement(sql);
		pst.execute();
		System.out.println("添加数据成功");
		
	} catch (Exception e) {
		
		System.out.println(e);
	}
}
```

### 更新和删除数据

hive是一个数据管理仓库，一般是不用于更新和删除数据的

如果需要支持updata和delete操作需要先在`hive-site.xml`添加如下属性，但是也仅仅支持事务表`ORC`，在建表的时候如果需要这个表支持这两个操作，需要设置参数`"transactional"="true"`，注意此操作不可逆

```xml
<property>

    <name>hive.support.concurrency</name>

    <value>true</value>

</property>

<property>

    <name>hive.enforce.bucketing</name>

    <value>true</value>

</property>

<property>

    <name>hive.exec.dynamic.partition.mode</name>

    <value>nonstrict</value>

</property>

<property>

    <name>hive.txn.manager</name>

    <value>org.apache.hadoop.hive.ql.lockmgr.DbTxnManager</value>

</property>

<property>

    <name>hive.compactor.initiator.on</name>

    <value>true</value>

</property>

<property>

    <name>hive.compactor.worker.threads</name>

    <value>1</value>

</property>
```


### 删除表

```java
// 删除表
public void dropTable(Connection con, String tableName, String dbName) {
	if (dbName != null) {
		dbName += ".";
	}else {
		dbName = "";
	}
	String sql = "drop table " + dbName + tableName;
	try {
		PreparedStatement pst = con.prepareStatement(sql);
		pst.execute();
		System.out.println("删除表" + tableName + "成功");
	} catch (Exception e) {
		
		System.out.println(e);
	}
}
```

### 查询

```java
// 查询
public void select(Connection con, String tableName, String dbName) {
	
	if (dbName != null) {
		dbName += ".";
	}else {
		dbName = "";
	}
	String sql = "select * from " + dbName + tableName;
	try {
		// 将 sql 发送到数据库，预编译
		PreparedStatement pst = con.prepareStatement(sql);
		// 接收查询结果
		ResultSet res = pst.executeQuery();
		// 输出结果
		System.out.println("表的内容");
		while (res.next()) {
			System.out.println(res.getString(1) + '\t' + res.getString(2));
		}	
	} catch (Exception e) {
		
		System.out.println(e);
	}
	
}
```

### 释放资源

```java
// 释放资源
public void clc(ResultSet res, Connection con, PreparedStatement pst) {
	try {
		if (res != null) {
			res.close();
		}
		if (con != null) {
			con.close();
		}
		if (pst != null) {
			pst.close();
		}
	} catch (Exception e) {
		
		System.out.println(e);
	}
}
```

## Java的编写

### test

```java
package com.example;

import java.sql.*;

public class Test {	
	// 查看数据库
	public void showDatabases(Connection con) {
		String sql = "show databases";
		try {
			PreparedStatement pst = con.prepareStatement(sql);
			ResultSet res = pst.executeQuery();
			System.out.println("现有的数据库");
			int i = 1;
			while (res.next()) {
				System.out.println(i + "\t" + res.getString(1));
				i += 1;
			}
		} catch (Exception e) {
			System.out.println(e);
		}
	}

	// 查看表
	public void showTables(Connection con, String dbName) {
		
		String sql_1 = "use " + dbName;
		String sql = "show tables";
		try {
			if (dbName != null) {
				PreparedStatement pst_1 = con.prepareStatement(sql_1);
				pst_1.execute();
			}
			PreparedStatement pst = con.prepareStatement(sql);
			ResultSet res = pst.executeQuery();
			int i = 1;
			System.out.println("数据库" + dbName + "的所有表");
			while (res.next()) {
				System.out.println(i + "\t" + res.getString(1));
				i++;
			}
		} catch (Exception e) {
			// TODO: handle exception
			System.out.println(e);
		}
	}
	
	// 创建表
	public void createTable(Connection con, String tableName, String dbName) {
		if (dbName != null) {
			dbName += ".";
		}else {
			dbName = "";
		}
		String  sql = "create table " + dbName + tableName + 
				"(id int, name string)";
		try {
			PreparedStatement pst = con.prepareStatement(sql);
			pst.executeUpdate();
			System.out.println("表" + tableName + "创建成功");
		} catch (Exception e) {
			System.out.println(e);
		}
	}
	
	// 添加数据
	public void insertData(Connection con, String tableName, String dbName, String id, String name) {
		if (dbName != null) {
			dbName += ".";
		}else {
			dbName = "";
		}
		String sql = "insert into " + dbName + tableName + " values(" + id + ",'" + name + "')";
		
		try {
			PreparedStatement pst = con.prepareStatement(sql);
			pst.execute();
			System.out.println("添加数据成功");
			
		} catch (Exception e) {
			
			System.out.println(e);
		}
	}
	
	// 删除表
	public void dropTable(Connection con, String tableName, String dbName) {
		if (dbName != null) {
			dbName += ".";
		}else {
			dbName = "";
		}
		String sql = "drop table " + dbName + tableName;
		try {
			PreparedStatement pst = con.prepareStatement(sql);
			pst.execute();
			System.out.println("删除表" + tableName + "成功");
		} catch (Exception e) {
			
			System.out.println(e);
		}
	}
	
	// 查询
	public void select(Connection con, String tableName, String dbName) {
		
		if (dbName != null) {
			dbName += ".";
		}else {
			dbName = "";
		}
		String sql = "select * from " + dbName + tableName;
		try {
			// 将 sql 发送到数据库，预编译
			PreparedStatement pst = con.prepareStatement(sql);
			// 接收查询结果
			ResultSet res = pst.executeQuery();
			// 输出结果
			System.out.println("表的内容");
			while (res.next()) {
				System.out.println(res.getString(1) + '\t' + res.getString(2));
			}	
		} catch (Exception e) {
			
			System.out.println(e);
		}
		
	}
	
	// 释放资源
	public void clc(ResultSet res, Connection con, PreparedStatement pst) {
		try {
			if (res != null) {
				res.close();
			}
			if (con != null) {
				con.close();
			}
			if (pst != null) {
				pst.close();
			}
		} catch (Exception e) {
			
			System.out.println(e);
		}
	}
	
}

```

### main

```java
package com.example;

import java.sql.*;
import java.util.Iterator;

import com.example.Test;

public class test_main {
	public static void main(String[] args) throws Exception {
		
		// 连接
		Class.forName("org.apache.hive.jdbc.HiveDriver");
		Connection con = DriverManager.getConnection("jdbc:hive2://YOUR_IP:10000", "YOUR_USER", "YOUR_PASSWORD");
		System.out.println("连接数据库成功");
		
		Test test = new Test();
		
		// 查看数据现有的数据库
		test.showDatabases(con);
		
		// 查看test的所有表
		test.showTables(con, "test");
		
		// 创建表
		test.createTable(con, "test_01", "test");
		
		// 添加数据
		String data[][] = {
				 {"1", "zs"}, 
				 {"2", "ls"}, 
				 {"3", "ww"}, 
				 {"4", "zl"}, 
				 {"5", "qtw"}};
		 for (int i = 0; i < data.length; i++) {
			 test.insertData(con, "test_01", "test", data[i][0], data[i][1]);
		 }
		 
		// 查看表数据
		test.select(con, "test_01", "test");
		 
		// 删除表
		test.dropTable(con, "test_01", "test");
		 
		// 查看表
		test.showTables(con, "test");

		// 释放资源
		test.clc(null, con, null);
	}
}
```



## 结果演示

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220429224237.png)