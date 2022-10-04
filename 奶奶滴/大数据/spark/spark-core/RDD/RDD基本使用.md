## 创建RDD

### 从内存中创建

使用`makeRDD`或`parallelize`方法

`makeRDD`底层会调用`parallelize`

```scala
// 1、建立和 spark 框架的连接   类似 jdbc    local[*]    * 代表线程数
val sparkConf = new SparkConf().setMaster("local[*]").setAppName("WordCount")  
val sc = new SparkContext(sparkConf)  
  
val rdd1 = sc.makeRDD(List(1, 2, 3))  
val rdd2 = sc.parallelize(List(1, 2, 3))
```

### 从外部文件读取

可以通过本地文件、hdfs、hbase等

path路径默认是当前环境的根路径为基准

也可以是hdfs路径等`hdfs://master:9000/....`

```scala
val sparkConf = new SparkConf().setMaster("local[*]").setAppName("WordCount")  
val sc = new SparkContext(sparkConf)  
  
val lines = sc.textFile("data/WordCount/*.txt", 3) // : RDD[String]
```

上述的`textFile`每次读取一行，返回字符串，不区分数据来源

可以通过`wholeTextFiles`方法，此方法返回一个元组`(文件路径, 数据)`，这样可以区分每个数据来源的文件

### 通过其他RDD生成新的RDD

```scala
val sparkConf = new SparkConf().setMaster("local[*]").setAppName("WordCount")  
val sc = new SparkContext(sparkConf)  
  
val lines = sc.textFile("data/WordCount/*.txt", 3) // : RDD[String]  
  
val newRDD = lines.flatMap(_.split(" "))   // 生成新的RDD
```

### 通过new直接构造

这种方法一般只用于spark内部

## 并行度与分区

### 设置默认分区

在读取文件时如果没有给定第二个参数（分区数），那么就会使用默认分区，默认分区可以自定义

```scala
val sparkConf = new SparkConf().setMaster("local[*]").setAppName("WordCount")  
sparkConf.set("spark.default.parallelism", "5")  
val sc = new SparkContext(sparkConf)
```

分区算法

```scala
def positons(length: Long, numSlices: Int): Iterator[(Int, Int)] = {
  (0 unitl numSlices).iterator.map { i => 
    val start = ((i * length) / numSlices).toInt
    val end = (((i + i) * length) / numSlices).toInt
    (start, end) 
  }
}
```

例如`val lines = sc.makeRDD(List(1,2,3,4,5), 3)`

那么会得到

```
0 => (0, 1)   List(1)
1 => (1, 3)   List(2, 3)
2 => (3, 5)   List(4, 5)
```

