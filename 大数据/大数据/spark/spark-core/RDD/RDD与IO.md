RDD与IO类似，但是RDD中间是不保存数据的，IO可以临时保存一部分数据

RDD的流程与IO的流程大致相同

例如：对于[词频统计](../../示例/单词计数.md)

```scala
val lines = sc.textFile("data/WordCount/*.txt", 3) // : RDD[String]  
 
lines.flatMap(_.split(" ")) // map + flatten  
  .map((_, 1)) //  word => (word, 1)  
  .reduceByKey(_ + _)  
  .collect()  
  .foreach(println) // println
```


![](http://www.droliz.cn/markdown_img/Pasted%20image%2020221002002155.png)


（'hello', 1）  1 
re  (1, 1, ,11,)