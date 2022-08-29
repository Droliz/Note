---
title: 结合java和phoenix操作hbase
注意: 所有的冒号是半角冒号，冒号后面有一个半角空格
zhihu-url: https://zhuanlan.zhihu.com/p/500169214
---
# 结合java和phoenix操作hbase

## 环境准备

### 配置Windows的hosts文件

参考[调用Java API操作HBase](https://zhuanlan.zhihu.com/p/496105738)

### 配置java项目
同[调用Java API操作HBase](https://zhuanlan.zhihu.com/p/496105738)创建java项目

将虚拟机上的phoenix的bin目录下的`phoenix-5.0.0-HBase-2.0-client.jar和phoenix-core-5.0.0-HBase-2.0.jar`传到java项目的lib目录下，并进行`build path`

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220415112137.png)
## 操作

### 连接phoenix

需要更改自己的虚拟机的ip地址

伪分布式只用写一个master，完全分布式要写全部节点，用`,`隔开

```java
private Connection conn;
private Statement stat;
private ResultSet rs;

// 开启连接
public void init() {
	try {
		Class.forName("org.apache.phoenix.jdbc.PhoenixDriver");
		conn = DriverManager.getConnection("jdbc:phoenix:IP地址:2181");
		stat = conn.createStatement();
		System.out.println("连接成功!");
	} catch (Exception e) {
		System.out.println(e);
	}
}
```

### 新建表

注意sql语法上tablename和table是有空格的

```java
public void createTable(String tableName) {
	String sql = "create table " + tableName 
			+ "( id unsigned_int primary key,"
			+ "name varchar,"
			+ "age unsigned_int)";
	// 执行
	try {
		stat.execute(sql);
		System.out.println("创建表成功");
	} catch (Exception e) {
		// handle exception
		System.out.println(e);
	}
}
```

注意：数据类型是不支持int类型的

|       类型        | 长度 |
| :---------------: | :--- |
|   unsigned_int    | 4    |
|   unsigned_long   | 8    |
| unsigned_tinyint  | 1    |
| unsigned_smallint | 2    |
|  unsigned_float   | 4    |
|  unsigned_double  | 8    |

具体参考[Phoenix的数据类型](https://blog.csdn.net/yuanhaiwn/article/details/82146651)

### 添加数据

`upsert into`相当于`insert into`

```java
public void upsertData(String tableName) {
	ArrayList<String> dataList = new ArrayList<String>();

	dataList.add("upsert into " + tableName + " values(1,'zhangsan',20)");
	dataList.add("upsert into " + tableName + " values(2,'lisi',25)");
	dataList.add("upsert into " + tableName + " values(3,'wangwu',18)");

	for (String sql : dataList) {
		try {
			stat.execute(sql);
			conn.commit();
			System.out.println("插入数据成功");
		} catch (Exception e) {
			System.out.println(e);
		}
	}
}
```

### 删除数据

```java
public void deleteData(String tableName, int id) {
	// 根据id删除数据
	String sql = "delete from " + tableName + " where id = " + id;
	try {
		stat.execute(sql);
		conn.commit();
		System.out.println("删除数据成功");
	} catch (Exception e) {
		System.out.println(e);
	}
}
```

### 查询数据

```java
public void selectData(String tableName) {
	// 查询所有数据
	String sql = "select * from " + tableName;
	try {
		// 保存结果集
		rs = stat.executeQuery(sql);
		System.out.println("id" + "\t" + "name" + "\t" + "age");
		while (rs.next()) {

			System.out.println(rs.getInt("id") + "\t" + rs.getString("name") + "\t" + rs.getInt("age"));
		}
		conn.commit();
		System.out.println("查询数据成功");
	} catch (Exception e) {
		System.out.println(e);
	}
}
```

### 删除表

```java
public void dropTable(String tableName) {
	String sql = "drop table " + tableName;
	try {
		stat.execute(sql);
		conn.commit();
		System.out.println("删除表成功");
	} catch (Exception e) {
		System.out.println(e);
	}
}
```

### 关闭连接

```java
public void closeConn() {
	if (rs != null) {
		rs.close();
	}
	if (stat != null){
		
		stat.close();  
	}
	if (conn != null){
		conn.close();
	}
}
```

## 完全代码
### test

```java
package phoenix_JAVA_API;

import java.sql.*;
import java.util.ArrayList;


public class test {
	
	private Connection conn;
	private Statement stat;
	private ResultSet rs;
	
	// 开启连接
	public void init() {
		try {
			Class.forName("org.apache.phoenix.jdbc.PhoenixDriver");
			conn = DriverManager.getConnection("jdbc:phoenix:IP地址:2181");
			stat = conn.createStatement();
			System.out.println("连接成功!");
		} catch (Exception e) {
			// exception
			System.out.println(e);
		}
	}
	
	// 创建表
	public void createTable(String tableName) {
		String sql = "create table " + tableName 
				+ "( id unsigned_int primary key,"
				+ "name varchar,"
				+ "age unsigned_int)";
		// 执行
		try {
			stat.execute(sql);
			System.out.println("创建表成功");
		} catch (Exception e) {
			// handle exception
			System.out.println(e);
		}
	}
	
	// 增加数据
	public void upsertData(String tableName) {
		ArrayList<String> dataList = new ArrayList<String>();

		dataList.add("upsert into " + tableName + " values(1,'zhangsan',20)");
		dataList.add("upsert into " + tableName + " values(2,'lisi',25)");
		dataList.add("upsert into " + tableName + " values(3,'wangwu',18)");

		for (String sql : dataList) {
			try {
				stat.execute(sql);
				conn.commit();
				System.out.println("插入数据成功");
			} catch (Exception e) {
				System.out.println(e);
			}
		}
	}
	
	// 删除数据
	public void deleteData(String tableName, int id) {
		// 根据id删除数据
		String sql = "delete from " + tableName + " where id = " + id;
		try {
			stat.execute(sql);
			conn.commit();
			System.out.println("删除数据成功");
		} catch (Exception e) {
			System.out.println(e);
		}
	}
	
	// 查询数据
	public void selectData(String tableName) {
		// 查询所有数据
		String sql = "select * from " + tableName;
		try {
			// 保存结果集
			rs = stat.executeQuery(sql);
			System.out.println("id" + "\t" + "name" + "\t" + "age");
			while (rs.next()) {

				System.out.println(rs.getInt("id") + "\t" + rs.getString("name") + "\t" + rs.getInt("age"));
			}
			conn.commit();
			System.out.println("查询数据成功");
		} catch (Exception e) {
			System.out.println(e);
		}
	}
	
	// 删除表
	public void dropTable(String tableName) {
		String sql = "drop table " + tableName;
		try {
			stat.execute(sql);
			conn.commit();
			System.out.println("删除表成功");
		} catch (Exception e) {
			System.out.println(e);
		}
	}
	
	// 关闭连接
	public void closeConn() {
		try {
			if (rs != null) {
				rs.close();
			}
			if (stat != null){
				
				stat.close();  
			}
			if (conn != null){
				conn.close();
			}
			System.out.println("关闭成功");
		} catch (Exception e) {
			System.out.println(e);
		}
	}
	

}
```

### main
```java
package phoenix_JAVA_API;

import phoenix_JAVA_API.test;

public class main {
	// main
	public static void main(String[] args) {
		// TODO Auto-generated method stub
		test Test = new test();

		Test.init();
		
		Test.createTable("student");
		
		Test.upsertData("student");
		
		Test.selectData("student");
		
		Test.deleteData("student", 3);
		Test.selectData("student");
		
		Test.dropTable("student");
		
		Test.closeConn();
	}
}

```

## 结果

![](http://www.droliz.cn/markdown_img/Pasted%20image%2020220417182612.png)



