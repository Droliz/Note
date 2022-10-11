# RDD

## 概念

一个RDD就是一个分布式对象集合，本质上是一个只读的分区记录集合，每个RDD可以分成多个分区，每个分区就是一个数据集片段。RDD提供了一种高度受限的共享内存模型，**即RDD是只读的记录分区的集合**，不能直接修改，只能基于稳定的物理存储中的数据集来创建RDD，或者通过在其他RDD上执行确定的转换操作（如map、join和groupBy）而创建得到新的RDD

RDD提供了一组丰富的操作以支持常见的数据运算，分为“行动”（Action）和“转换”（Transformation）两种类型，前者用于执行计算并指定输出的形式，后者指定RDD之间的相互依赖关系。

两类操作的主要区别是，转换操作（比如map、filter、groupBy、join等）接受RDD并返回RDD，而行动操作（比如count、collect等）接受RDD但是返回非RDD（即输出一个值或结果）

## Transformation算子

### value 类型算子

#### map

将RDD中的每一个数据项映射为新的数据项

例如词频统计

```scala
val sparkConf = new SparkConf().setMaster("local").setAppName("WordCount")  
val sc = new SparkContext(sparkConf)  
  
val data = Array("hello", "hadoop", "hello", "spark")  
data.foreach(println)
val data_rdd = sc.makeRDD(data)    // 创建一个 RDD
data_rdd.map((_, 1)).foreach(println)  
```

将`"hello", "hadoop", "hello", "spark"`的数据通过map把每一个数据项从`word => (word, 1)`

![](../../../../../markdown_img/Pasted%20image%2020220927090907.png)

#### flatmap

`flatmap = map + flatten`

![](../../../../../markdown_img/Pasted%20image%2020220924144824.png)


```scala
def flatMap[B](f: (A) ⇒ GenTraversableOnce[B]): TraversableOnce[B]
```


* 泛型`[B]`
	* 最终要转换的集合的元素类型
* 参数`f: (A) ⇒ GenTraversableOnce[B]`
	* 传入一个函数对象函数的参数是集合的元素函数的返回值是一个集合
* 返回值`TraversableOnce[B]`
	* B类型的集合


对于如下的数据

```scala
val lines = List(  
  "Hadoop Hbase Hive Hadoop",  
  "Spark scala Java mysql"  
)
```

采用原始的先map后flatten

```scala
println(lines.map(_.split(" ")).flatten)  
// List(Hadoop, Hbase, Hive, Hadoop, Spark, scala, Java, mysql)
```

采用函数式编程flatMap

```scala
println(lines.flatMap(_.split(" ")))
// List(Hadoop, Hbase, Hive, Hadoop, Spark, scala, Java, mysql)
```

flatmap事实上是将一个元素对应出想要的多个元素，最后全部扁平化到一个集合中，那么就有一个很有趣的，可以在源数据的基础上新增根据原数据运算出来的数据

```scala
def test(): Unit = {  
  val sparkConf=new SparkConf().setMaster("local[*]").setAppName("111")  
  val sc = new SparkContext(sparkConf)  
  
  val rdd = sc.makeRDD(List(1, 2, 3, 4, 5))  
  
  val newRdd = rdd.flatMap(item => {  
    List(item + 1, item + 2, item + 3)  
  })  
  
  val a = newRdd.collect()  
  println(a.mkString(","))  
}


// 2,3,4,3,4,5,4,5,6,5,6,7,6,7,8
```

#### mapPartitions

与map算子类似，但传入的参数是一个迭代器（一次接受所有），通过迭代器可以访问各数据项，效率要比map更高

例如map中的例子

```scala
data_rdd.mapPartitions(_.map((_, 1)))
```

以分区为单位对数据进行处理，这里的处理是指可以进行任意的处理，哪怕是过滤数据

相较于map，mapPartitions可以计算每个分区的最大值

#### mapPartitionsWithIndex

类似mapPartition，比mapPartitions多一个参数来表示分区号

可以结合模式匹配，来对不同分区进行不同的操作

```scala
val data = Array("hello", "hadoop", "hello", "spark")  
val data_rdd = sc.makeRDD(data)  
data_rdd.foreach(println)  
data_rdd.mapPartitionsWithIndex((index, data) => {  
  index match {  
    case 1 => data.map((_, 2))  
    case _ => data.map((_, 1))  
    // ......  
  }  
}).foreach(println)
```

#### glom

将RDD中每一个分区变成一个数组

```scala
val data = Array("hello", "hadoop", "hello", "spark")  
val data_rdd = sc.makeRDD(data)  
data_rdd.glom().foreach(data => println(data.mkString(",")))
```

#### groupBy

根据指定的规则进行分组，分区默认不变，数据会被打乱（shuffle）。极限情况下，数据可能会被分到同一个分区中。

一个分区可以有多个组，一个组只能在一个分区中

```scala
val data = Array("hello", "hadoop", "hello", "spark")  
val data_rdd = sc.makeRDD(data)  
val groupRdd = data_rdd.groupBy(word => word)  
groupRdd.collect().foreach(println)  
  
sc.stop()
```

```txt
(hadoop,CompactBuffer(hadoop))
(spark,CompactBuffer(spark))
(hello,CompactBuffer(hello, hello))
```

#### filter

根据指定的规则进行筛选过滤，符合规则的数据保留，不符合的丢弃

当数据进行筛选过滤后，分区不变，但是分区内数据可能不均衡，导致数据倾斜

```scala
val data = Array(1, 2, 3, 4, 5)  
val data_rdd = sc.makeRDD(data)  
data_rdd.filter(_ % 2 == 1)  
  .collect()  
  .foreach(println)
```

#### sample

根据指定规则从数据集中采样数据。通过它可以找到数据倾斜的key

```scala
val data = Array(1, 2, 3, 4, 5)  
val data_rdd = sc.makeRDD(data, 2)  
println(data_rdd.sample(  
  withReplacement = true,  
  0.8)  
  .collect()  
  .mkString(","))
```

- `withReplacement`：表示抽出样本后是否在放回去，true表示会放回去，这也就意味着抽出的样本可能有重复
- `fraction`：抽出多少，这是一个double类型的参数0-1之间百分制

#### distinct

将数据集中的数据去重。使用分布式处理方式实现，与内存集合使用HashSet去重方式不同

```scala
val data = Array(1, 2, 3, 4, 5, 4, 1, 2)  
val data_rdd = sc.makeRDD(data, 2)  
data_rdd.distinct()  
  .collect()  
  .foreach(println)
```

#### coalesce

根据数据量缩减分区，用于大数据集过滤后，提高小数据集的执行效率

当Spark程序中存在过多的小任务时，可以通过coalesce合并分区，减少分区个数，进而减少任务调度成本

```scala
val data = Array(1, 2, 3, 4, 5, 4, 1, 2)  
val data_rdd = sc.makeRDD(data, 2)  
data_rdd.coalesce(2, shuffle = true)
```

#### sortBy

根据规则排序，默认升序，设置第二个参数改变排序方式。默认情况下，不会改变分区个数，但是中间存在shuffle处理

```scala
val data = Array(1, 2, 3, 4, 5, 4, 1, 2)  
val data_rdd = sc.makeRDD(data, 2)  
data_rdd.sortBy(num => num, ascending = false, 2)  
  .collect().foreach(println)
```

### 双 value 算子

#### intersection

求两个RDD的交集

```scala
val newRDD = rdd1.intersection(rdd2)
```

#### union

求两个RDD的并集

```scala
val newRDD = rdd1.union(rdd2)
```

#### subtract

求两个RDD的差集

```scala
val newRDD = rdd1.subtract(rdd2)
```

#### zip

同python的zip函数，将两个RDD通过键值对的形式合并

```scala
val newRDD = rdd1.zip(rdd2)
```

intersection，union和subtract要求两个RDD中的数据类型保持一致

zip不要求数据类型一致，但是要求分区的个数以及每个分区上数据个数一致

### K-V算子

#### partitionBy

将数据按照指定artitioner重新进行分区，默认的分区器是HashPartitioner

```scala
data_rdd.partitionBy(new HashPartitioner(2)) // 分区数量 2 
  .saveAsTextFile("data/WordCount/output")   
```

#### reduceByKey

将数据按照相同的key对value进行聚合

```scala
val data = Array("hadoop", "spark", "java", "java", "spark", "kafka", "hello")  
val data_rdd = sc.makeRDD(data).map((_, 1))   // 处理成 K-V
data_rdd.reduceByKey(_ + _)   // (v1, v2) => {v1 + v2}  
  .collect()  
  .foreach(println)
```

#### groupByKey

将数据按照相同的key对value进行分组，形成一个对**偶元祖**

元组中的第一个元素就是key，第二个元素就是相同key的value集合

```scala
val data = Array("hadoop", "spark", "java", "java", "spark", "kafka", "hello")  
val data_rdd = sc.makeRDD(data).map((_, 1))  
data_rdd.groupByKey()     
  .collect()  
  .foreach(println)
```

```txt
(hadoop,CompactBuffer(1))
(spark,CompactBuffer(1, 1))
(kafka,CompactBuffer(1))
(hello,CompactBuffer(1))
(java,CompactBuffer(1, 1))
```

#### aggregateByKey

使用aggregateByKey算子，可以先对分区内进行seqOp处理，对于不同分区使用combOp处理

```scala
val data = List(("a",1),("a",2),("a",3),("a",4))  
val data_rdd = sc.makeRDD(data, 2)  
data_rdd.aggregateByKey(0)(   // zeroValue：给每一个分区中的每一个key一个初始值  
  (x, y) => math.max(x,y),   // seqOp：函数用于在每一个分区中用初始值逐步迭代value  
  (x, y) => x + y           // combOp：函数用于合并每个分区中的结果  
).collect()  
  .foreach(println)
```

如果一类数据只在一个分区中出现，则combOp运算时直接忽略此类数据的计算

#### foldByKey

aggregateByKey的简化操作，此时分区内和分区间的计算规则一样

```scala
val data = List(("a",1),("a",2),("a",3),("a",4))  
val data_rdd = sc.makeRDD(data, 2)  
data_rdd.foldByKey(0)(   // zeroValue：给每一个分区中的每一个key一个初始值  
  (x, y) => x + y                // seqOp  combOp  
).collect()  
  .foreach(println)
```

#### combineByKey

对于相同的key，将value组合为一个集合

combineByKey 需要三个参数 
- 第一个参数表示：给定每个组（value）第一个值一个初始值（函数）
- 第二个参数表示：分区内的计算规则 ，combinbe聚合逻辑
- 第三个参数表示：分区间的计算规则，reduce端聚合逻辑

```txt
// createCombiner: V => C,   
// mergeValue: (C, V) => C, 
// mergeCombiners: (C, C) => C
```


```scala
val data = List(("a",1),("a",2),("a",3),("a",4),("b",2),("b",2))  
val data_rdd = sc.makeRDD(data, 2)  
data_rdd.combineByKey(  
  v => (v, 1),   // C = (v, 1)  
  (t:(Int, Int), v) =>     // (C, V)  
    (t._1 + v, t._2 + 1),  // 返回 C = (sum, count)  
    (t1:(Int, Int), t2:(Int, Int)) =>  // 同一个key不同分区的 C = (Sum, Count)
    (t1._1 + t2._1, t1._2 + t2._2)   // 求和  
).collect()  
  .foreach(println)
```


#### join

在类型为（K，V）和（K，W）的RDD上调用，返回一个相同的key对应的所有元素连接在一起的（K，（V，W））的RDD

```scala
val rdd = sc.makeRDD(List(("a",1),("b",2),("c",3),("d",4)))  
val rdd2 = sc.makeRDD(List(("a",5),("a",6),("e",8),("c",7)))  
  
rdd.join(rdd2).collect().foreach(println)
```

```txt
(a,(1,5))
(a,(1,6))
(c,(3,7))
```

#### sortByKey

在一个(K,V)的RDD上调用，K必须实现ordered接口，返回一个按照key进行排序的(K,V)的RDD

```scala
val rdd = sc.makeRDD(List(("a",1),("b",2),("c",3),("d",4)))  
//按照key对rdd中的元素进行排序，默认升序  
rdd.sortByKey().collect().foreach(println)  
//降序  
rdd.sortByKey(ascending = false)  
  .collect()  
  .foreach(println)
```

#### mapValues

针对于(K,V)形式的类型只对V进行操作

```scala
val rdd = sc.makeRDD(List(("a",1),("b",2),("c",3),("d",4)))  
rdd.mapValues("value=" + _).collect().foreach(println)
```

#### cogroup

相同的key，value分组后连接起来

```scala
val rdd = sc.makeRDD(List(("a",1),("b",2),("c",3),("d",4)))  
val rdd2 = sc.makeRDD(List(("a",5),("a",6),("e",8),("c",7)))  
// cogroup ： group + connect  (分组，连接)  
// 可以有多个参数  
rdd.cogroup(rdd2).collect().foreach(println)
```

```txt
(a,(CompactBuffer(1),CompactBuffer(5, 6)))
(b,(CompactBuffer(2),CompactBuffer()))
(c,(CompactBuffer(3),CompactBuffer(7)))
(d,(CompactBuffer(4),CompactBuffer()))
(e,(CompactBuffer(),CompactBuffer(8)))
```



## Action算子

### reduce
聚合RDD中的所有数据，先聚合分区内数据，在聚合分区间数据。

### collect
采集，该方法会将不同分区间的数据按照分区顺序采集到Driver端，形成数组。

### count
统计数据个数。

### first
获取RDD中的第一个元素。

### take
获取RDD前n个元素组成的数组。

### takeOrdered
获取RDD排序后的前n个元素组成的数组。

### aggregate
将每个分区里面的元素通过分区内逻辑和初始值进行聚合，然后用分区间逻辑和初始值(zeroValue)进行操作。注意:分区间逻辑再次使用初始值和aggregateByKey是有区别的。

### fold
折叠操作，aggregate的简化操作，分区内逻辑和分区间逻辑相同。

### countByValue
统计每个value的个数

### countByKey
统计每种key的个数。

### foreach
遍历RDD中每一个元素。

### save

- （1）saveAsTextFile(path)保存成Text文件  
- （2）saveAsSequenceFile(path) 保存成Sequencefile文件  
- （3）saveAsObjectFile(path) 序列化成对象保存到文件

### 示例

```scala
val conf = new SparkConf().setMaster("local[*]").setAppName("Operator")  
    val sc = new SparkContext(conf)  
//    val rdd = sc.makeRDD(List(1,2,3,4),2)  
// 行动算子，其实就是触发作业执行的方法  
// 底层代码调用的是环境对象中 runJob 方法，调用dagScheduler的runJob方法创建ActiveJob，并提交执行。  

// reduce  
//    val i = rdd.reduce(_ + _)    
//    println(i)  

// collect : 采集，该方法会将不同分区间的数据按照分区顺序采集到Driver端，形成数组  
//    val ints = rdd.collect()  
//    ints.foreach(println)  

// count : 数据源中数据个数  
//    val l = rdd.count()  
//    println(l)  
// first : 获取数据源中数据的第一个  
//    val i = rdd.first()  
//    println(i)  
// take : 获取数据源中数据的前N个  
//    val ints = rdd.take(2)  
//    println(ints.mkString(","))  
// takeOrdered : 数据先排序，再取N个，默认升序排序，可以使用第二个参数列表（比如 ： Ordering.Int.reverse）实现倒序功能  
//    val rdd1 = sc.makeRDD(List(4,3,2,1))  
//    val ints1 = rdd1.takeOrdered(2)(Ordering.Int.reverse)    
//    println(ints1.mkString(","))  
	//aggregate    
//    val rdd: RDD[Int] = sc.makeRDD(List(1, 2, 3, 4), 8)    
//    println(rdd.aggregate(10)(_ + _, _ + _))  
//fold 是aggregate的简化版  
//    rdd.fold(10)(_+_)  
  
//countByKey 统计每种key出现的次数  
//    val rdd: RDD[(Int, String)] = sc.makeRDD(List((1, "a"), (1, "a"), (1, "a"), (2, "b"), (3, "c"), (3, "c")))  
//    println(rdd.countByKey())    
//    val intToLong = rdd.countByValue()    
//    println(intToLong)  
  
    // save    
    val rdd = sc.makeRDD(List(("a",1),("a",2),("a",3),("b",4)),2)  
    rdd.saveAsTextFile("data/output")  
    rdd.saveAsObjectFile("data/output1")  
    // saveAsSequenceFile 方法要求数据的格式必须为 K-V 键值对类型  
    rdd.saveAsSequenceFile("data/output2")  
    sc.stop()
```

## 序列化

RDD算子中传递的函数是会包含闭包操作，那么就会进行闭包检测

在scala中类的构造参数是类的属性，构造参数需要进行闭包检测，等同于类需要闭包检查，要么创建样例类`case`，要么混入`Serializable`，或者创建局部变量

```scala
def main(args: Array[String]): Unit = {  
  // 1、建立和 spark 框架的连接   类似 jdbc    local[*]    * 代表线程数  
  val sparkConf = new SparkConf().setMaster("local[*]").setAppName("WordCount")  
  val sc = new SparkContext(sparkConf)  
  
  val rdd = sc.makeRDD(Array("hadoop", "good"))  
  
  val search = new search("h")  
  
  search.getMatch1(rdd)   // Task not serializable  
  search.getMatch2(rdd)    // Task not serializable  
    .collect()  
    .foreach(println)  
  
  sc.stop()  
  
}  
  
case class search(query: String) {  
  def isMach(s: String): Boolean = {  
    s.contains(query)  
  }  
  
  // 函数序列化  
  def getMatch1(rdd: RDD[String]): RDD[String] = {  
    rdd.filter(isMach)  
  }  
  
  // 属性序列化  
  def getMatch2(rdd: RDD[String]): RDD[String] = {  
    // val s = query    也可以解决序列化问题    
rdd.filter(_.contains(query))  // _contains(s)  
  }  
}
```

java的序列化可以序列化任何类，但是字节多传输效率低spark2.x支持另一种序列化框架`kryo`，比较轻巧，即便使用`kryo`也需要继承`Serializable`接口

RDD在shuffle数据时，简单数据类型、数组、字符串以及在spark内部使用Kryo序列化

```scala

```