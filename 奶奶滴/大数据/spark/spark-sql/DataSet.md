## DataSet

DataSet 具有强类型的数据集合，需要提供对应的数据类型信息

## 创建DataSet

**通过样例类创建**

```scala
def main(args: Array[String]): Unit = {  

	val spark = SparkSession  
	  .builder()  
	  .master("local[*]")  
	  .appName("test")  
	  .getOrCreate()  
	
	import spark.implicits._   // 导入隐式转换
	
	val list = List(Person("zs", 18), Person("ls", 20))  
	
	val ds = list.toDS()  // 类型转换
	
	ds.show()  
	spark.close()
}  

case class Person(name: String, age: Long)
```

普通的序列也是可以通过上述方法转为DS

**通过RDD转换**

```scala
// 1、建立和 spark 框架的连接   类似 jdbc    local[*]    * 代表线程数  
val sparkConf = new SparkConf().setMaster("local[*]").setAppName("WordCount")  
val sc = new SparkContext(sparkConf)  
val spark = SparkSession.builder()  
  .config(sparkConf)  
  .getOrCreate()  
  
import spark.implicits._  
val lines = sc.textFile("data/WordCount/*.txt", 3) // : RDD[String]  
  
val ds = spark.createDataset(lines)  
// val ds = lines.toDS()
ds.show()
```

**通过DF创建**

