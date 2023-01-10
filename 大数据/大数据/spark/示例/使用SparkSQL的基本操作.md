## 准备

在`spark/examples/src/main/resources/`目录下有多个文件

选取其中的`people.json、people.txt`文件放到spark-sql项目根目录下的`data/spark-sql/`目录下

如果是在虚拟机上直接使用`spark-shell`那么不需要以上操作

## 基本操作

### 连接spark

```scala
val spark = SparkSession  
  .builder()  
  .master("local[*]")   // 如果是连接虚拟机上的   spark://IP:7077
  .appName("sparkSql")  
  .getOrCreate()
```

### 读取文件

```scala
val personDF = spark.read.text("data/spark_sql/person.txt")  
personDF.show()
```

结果

```txt
+-------------+
|        value|
+-------------+
|   1,jacky,30|
|    2,lucy,28|
|    3,lisi,24|
|4,zhangsan,25|
|   5,lucas,26|
|    6,小明,32|
|    7,小红,36|
|    8,小龙,36|
+-------------+
```

### 读取json文件

**显示全部**
```scala
val df = spark.read.json("data/spark_sql/person.json")  
df.show()  
```

```txt
+----+-------+
| age|   name|
+----+-------+
|null|Michael|
|  30|   Andy|
|  19| Justin|
+----+-------+
```

**显示指定的列**
```
df.select("name").show  
```

```txt
+-------+
|   name|
+-------+
|Michael|
|   Andy|
| Justin|
+-------+
```

**打印schema信息**
```   
df.printSchema
```

```txt
root
 |-- age: long (nullable = true)
 |-- name: string (nullable = true)
```

**选择多列和条件过滤** 

```scala
df.select(df("name"),df("age")).show  
df.filter(df("age") > 20).show
```

```txt
+-------+----+
|   name| age|
+-------+----+
|Michael|null|
|   Andy|  30|
| Justin|  19|
+-------+----+

+---+----+
|age|name|
+---+----+
| 30|Andy|
+---+----+
```

**分组聚合和排序**

```scala
df.groupBy("age").count().show  
df.sort(df("age").desc).show
```

```txt
+----+-----+
| age|count|
+----+-----+
|  19|    1|
|null|    1|
|  30|    1|
+----+-----+

+----+-------+
| age|   name|
+----+-------+
|  30|   Andy|
|  19| Justin|
|null|Michael|
+----+-------+
```

**多列排序**

```scala
df.sort(df("age").desc,df("name").asc).show
```

```txt
+----+-------+
| age|   name|
+----+-------+
|  30|   Andy|
|  19| Justin|
|null|Michael|
+----+-------+
```

**对列进行重命名**

```scala  
df.select(df("name").as("username"),df("age")).show
```

```txt
+--------+----+
|username| age|
+--------+----+
| Michael|null|
|    Andy|  30|
|  Justin|  19|
+--------+----+
```


### 使用sql语句

先要将fd注册成表，再通过sql语句查询这个表

**注册成表**

```scala
df.createTempView("person")
```

**调用sql方法查询**

基本查询

```scala
spark.sql("select * from person").show
spark.sql("select name from person").show
spark.sql("select name,age from person").show
```

```txt
+----+-------+
| age|   name|
+----+-------+
|null|Michael|
|  30|   Andy|
|  19| Justin|
+----+-------+

+-------+
|   name|
+-------+
|Michael|
|   Andy|
| Justin|
+-------+

+-------+----+
|   name| age|
+-------+----+
|Michael|null|
|   Andy|  30|
| Justin|  19|
+-------+----+
```

条件查询

```scala
spark.sql("select * from person where age > 30").show
spark.sql("select count(*) from person where age > 30").show
spark.sql("select age,count(*) from person group by age").show
```

```txt
+---+----+
|age|name|
+---+----+
+---+----+

+--------+
|count(1)|
+--------+
|       0|
+--------+

+----+--------+
| age|count(1)|
+----+--------+
|  19|       1|
|null|       1|
|  30|       1|
+----+--------+
```

条件查询

```scala
spark.sql("select age,count(*) as count from person group by age").show
spark.sql("select * from person order by age desc").show
// 执行模糊查询
spark.sql("select * from person where name like '%e%'").show
spark.sql("select sum(age) from person ").show
```

```txt
+----+-----+
| age|count|
+----+-----+
|  19|    1|
|null|    1|
|  30|    1|
+----+-----+

+----+-------+
| age|   name|
+----+-------+
|  30|   Andy|
|  19| Justin|
|null|Michael|
+----+-------+

+----+-------+
| age|   name|
+----+-------+
|null|Michael|
+----+-------+

+--------+
|sum(age)|
+--------+
|      49|
+--------+
```