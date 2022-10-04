## DataFrame

DataFrame也有transformation和Action操作

## Schema

schema就是结构表的列名与列类型

**Schema 的两种定义方式：**

-   使用 StructType 定义，是一个样例类，属性为 StructField 的数组
-   使用 StructField 定义，同样是一个样例类，有四个属性，其中字段名称和类型为必填

```scala
val schema = StructType([
	StructField('name', String),
	StructField('name', String)
])
```

## Row

DataFrame中每条数据封装在Row中，Row表示每行数据

## 创建DataFrame

SparkSession是创建DataFrame和执行SQL的入口

**1、通过spark数据源创建**

例如`text、json`等

读取json要求每行都是`{"username":"zs", ...}`的形式

```scala
val spark = SparkSession  
  .builder()  
  .master("local[*]")  
  .appName("test")  
  .getOrCreate()  
  
val df = spark.read.json("data/spark_sql/person.json")  
df.show()
```

**2、从一个存在的RDD进行转换**

RDD中有一个方法`toDF()`这个方法要求提供结构

```scala
val df = rdd.toDF(字段1，字段2....)
```

```scala
val sparkConf = new SparkConf().setMaster("local[*]").setAppName("WordCount")  
val sc = new SparkContext(sparkConf)  
val spark = SparkSession.builder()  
  .config(sparkConf)  
  .getOrCreate()  

import spark.implicits._  
val lines = sc.textFile("data/WordCount/*.txt", 3) // : RDD[String]  
val rowRDD = lines.flatMap(_.split(" ")).map((_, 1))  
//    rowRDD.foreach(println)  
val df = rowRDD.toDF("word", "count")  
df.show()
```

DF同样也有属性`rdd`转换为RDD

```scala
val rdd = df.rdd
```

**3、从Hive Table进行查询返回**

## 使用sql语句

需要先对df创建一个临时视图

```scala
df.createTempView("tableName")  // 多次创建可能会重复
dfcreateOrReplaceTempView("tableName")   // 解决上述问题
```

使用`df.sql()`进行sql查询

此方法中可以写任意的sql语句

```scala
df.sql("select * from tableName").show()
```

需要注意如果查询语句**涉及到的表中有其他的连接Session连接创建的临时表**，那么查询会报错，此时需要将跨`Session`查询的表创建为全局`df.createGlobalTempView()`，同时使用时也需要将名字改为`global_tableName`

## DSL

通过一些方法封装sql语法，可以简化sql语句（不需要创建view）

例如：

```scala
df.select("name").show  
df.select(df("name"),df("age")).show  
df.filter(df("age") > 20).show
df.select("*").show
```


如果涉及到运算，例如age + 1，那么需要`df("age")`才能正确运算

命令行中使用`$"age"`或`'age`

如果想在IDEA中使用命令行的形式那么需要引入转换规则

```scala
import spark.implicits._   // spark 是 SparkSession 对象
```

```scala
df.select(df("age") + 1).show
```